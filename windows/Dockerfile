ARG WINDOWS_IMAGE=servercore
ARG WINDOWS_VERSION_BUILD=1809
ARG WINDOWS_VERSION=ltsc2019
FROM golang:1.19-windows${WINDOWS_IMAGE}-${WINDOWS_VERSION_BUILD} AS builder

ARG REWINGED_VERSION=0.3.0
ARG REWINGED_DOWNLOAD_HASH=deb6df1749dec36f26bb4edd4387dfb17e40358663cca903d703230bf15bddf5

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

WORKDIR $GOPATH\\src\\jantari

ENV TEMP=C:\\Windows\\TEMP

RUN Write-Host 'Downloading rewinged source...'; \
    \
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; \
    Invoke-WebRequest -Uri ("'https://github.com/jantari/rewinged/archive/refs/tags/v{0}.zip'" -f $Env:REWINGED_VERSION) \
        -OutFile ("'{0}\\rewinged.zip'" -f $Env:TEMP); \
    \
    Write-Host ('Verifying sha256 ({0}) ...' -f $Env:REWINGED_DOWNLOAD_HASH); \
    if ((Get-FileHash ("'{0}\\rewinged.zip'" -f $Env:TEMP) -Algorithm sha256).Hash -ne $Env:REWINGED_DOWNLOAD_HASH) { \
    Write-Host 'FAILED!'; \
    exit 1; \
    }; \
    \
    Expand-Archive -LiteralPath ("'{0}\\rewinged.zip'" -f $Env:TEMP) -DestinationPath  ("'{0}\\\\src\\jantari'" -f $Env:GOPATH);  \
    Rename-Item -Path ("'{0}\\\\src\\jantari\\rewinged-{1}'" -f $Env:GOPATH, $Env:REWINGED_VERSION) \
        -NewName ("'{0}\\\\src\\jantari\\rewinged'" -f $Env:GOPATH); \
    \
    Write-Host 'Complete.'

WORKDIR $GOPATH\\src\\jantari\\rewinged

RUN $time = Get-Date; \
    go build -o rewinged.exe \
    -ldflags ("'-X main.releaseMode=true -X main.version={0} -X main.commit=unknown -X \\\"main.compileTime={1}\\\"'" -f $Env:REWINGED_VERSION, $time) \
    .

FROM mcr.microsoft.com/windows/${WINDOWS_IMAGE}:${WINDOWS_VERSION}

USER ContainerAdministrator

COPY --from=builder ["C:/go/src/jantari/rewinged/rewinged.exe", "C:/rewinged.exe"]

RUN mkdir "%ProgramFiles%\\Rewinged" \
    && mkdir "%ProgramData%\\Rewinged" \
    && mkdir "%ProgramData%\\Rewinged\packages" \
    && move C:/rewinged.exe "%ProgramFiles%\\Rewinged" \
    && setx PATH "%PATH%;%ProgramFiles%\Rewinged" /m

USER ContainerUser

WORKDIR C:/ProgramData/Rewinged

ENTRYPOINT ["rewinged.exe"]
CMD ["-listen", ":8080", "-manifestPath", "C:/ProgramData/Rewinged/packages"]
