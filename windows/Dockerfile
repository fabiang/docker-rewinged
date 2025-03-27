# escape=`
ARG WINDOWS_IMAGE=servercore
ARG WINDOWS_VERSION_BUILD=1809
ARG WINDOWS_VERSION=ltsc2019
ARG GO_VERSION=1.24
FROM golang:${GO_VERSION}-windows${WINDOWS_IMAGE}-${WINDOWS_VERSION_BUILD} AS builder

ARG REWINGED_VERSION=0.7.1
ARG REWINGED_DOWNLOAD_HASH=b2fe4bdc8e44a30f93c4969dbe72aa9393abbfe57ad6d650a7cb07fe1ad90ec0

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

WORKDIR $GOPATH\src\jantari

ENV TEMP=C:\Windows\TEMP

RUN Write-Host 'Downloading rewinged source...'; `
    `
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; `
    Invoke-WebRequest -Uri ("'https://api.github.com/repos/jantari/rewinged/zipball/v{0}'" -f $Env:REWINGED_VERSION) `
        -OutFile ("'{0}\rewinged.zip'" -f $Env:TEMP); `
    `
    Write-Host ('Verifying sha256 ({0}) ...' -f $Env:REWINGED_DOWNLOAD_HASH); `
    if ((Get-FileHash ("'{0}\rewinged.zip'" -f $Env:TEMP) -Algorithm sha256).Hash -ne $Env:REWINGED_DOWNLOAD_HASH) { `
        Write-Host 'FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive -LiteralPath ("'{0}\rewinged.zip'" -f $Env:TEMP) `
        -DestinationPath  ("'{0}\src\jantari'" -f $Env:GOPATH); `
    Move-Item -Path ("'{0}\src\jantari\jantari-rewinged-*'" -f $Env:GOPATH) `
        -Destination ("'{0}\src\jantari\rewinged-{1}'" -f $Env:GOPATH, $Env:REWINGED_VERSION); `
    Rename-Item -Path ("'{0}\src\jantari\rewinged-{1}'" -f $Env:GOPATH, $Env:REWINGED_VERSION) `
        -NewName ("'{0}\src\jantari\rewinged'" -f $Env:GOPATH); `
    `
    Write-Host 'Complete.'

WORKDIR $GOPATH\src\jantari\rewinged

RUN $time = Get-Date; `
    go build -o rewinged.exe `
        -ldflags ("'-X main.releaseMode=true -X main.version={0} -X main.commit=unknown -X \\\"main.compileTime={1}\\\"'" -f $Env:REWINGED_VERSION, $time) `
        .

FROM mcr.microsoft.com/windows/${WINDOWS_IMAGE}:${WINDOWS_VERSION}

USER ContainerAdministrator

COPY --from=builder "C:\go\src\jantari\rewinged\rewinged.exe" "C:\rewinged.exe"

RUN mkdir "%ProgramFiles%\Rewinged" `
    && mkdir "%ProgramData%\Rewinged" `
    && mkdir "%ProgramData%\Rewinged\Packages" `
    && move C:\rewinged.exe "%ProgramFiles%\Rewinged" `
    && setx PATH "%PATH%;%ProgramFiles%\Rewinged" /m

USER ContainerUser

WORKDIR C:\ProgramData\Rewinged

ENTRYPOINT ["rewinged.exe"]
CMD ["-listen", ":8080", "-manifestPath", "C:\ProgramData\Rewinged\Packages"]
