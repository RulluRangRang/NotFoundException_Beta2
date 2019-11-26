# NotFoundException

## 0. �⺻ ����
### 1) Visual Studio Code ��ġ(https://code.visualstudio.com/)
### 2) .net core SDK ��ġ
### 3) Git ��ġ
    $ git config user.name "Github ����� �̸�"
    $ git config user.email <email �ּ�>
### 4) MinGW ��ġ(gcc) - optional
### 5) Docker ��ġ
* * * 

## 1. Visual Studio Code ����
1. C# Extension ��ġ
2. C/C++ Extension ��ġ
3. CMake Tools ��ġ
4. Dracula Official Theme ��ġ
5. Live Server Extension ��ġ
6. Visual Studio IntelliCode Extension ��ġ
7. Bootstrap 4, Font awesome 4, Font Awesome 5 Free & Pro snippets Extension ��ġ
8. Docker Extension ��ġ
* * *

## 2. �ֿ� ����Ű
1. Preview README.md : Ctrl + Shift + V
2. EXPLORER â ����/�ݱ� : Ctrl + B
3. ������ ����� : Ctrl + N
4. VSCode �� â ���� : Ctrl + Shift + N
5. ���� ����â �ݱ� : Ctrl + W
6. Extensions â ���� : Ctrl + Shift + X
7. â �¿� �и� : Ctrl + \
8. â �¿� ���� : Ctrl + 1 / 2
9. Explorer : Ctrl + Shift + E
10. VSCode Terminal : Ctrl + `
11. ��� ���� : Ctrl + K, S
12. ���� ã�� : Ctrl + S
13. Debug â �̵� : Ctrl + Shift + D
* * *

## 3. dotnet CLI
### 1) .sln ���� ����
    $ dotnet new sln -n NotFoundException
    $ dotnet sln add src\Web\Web.csproj

### 2) ������Ʈ ����
    $ dotnet new mvc -n Web -o "src\\Web"
    - ���� : -n ������Ʈ�� -o "������Ʈ �������� �����ġ"

### 3) Nuget �߰�
    $ dotnet add package <package-name>
    $ dotnet add "src\Web" package <package-name>

### 4) Nuget ����
    $ dotnet remove package <package-name>
    $ dotnet remove "src\Web" package <package-name>

* * *

## 4. Webpack ����
(���� : https://medium.com/@rich.wifidelity/asp-net-mvc-core-2-2-no-spa-with-webpack-4-jquery-bootstrap-babel-7-1f6584665566)

1. npm ��ġ
2. src\Web\ClientApp ���� �����
3. ClientApp> npm init -y
4. ClientApp> npm i -D @babel/core @babel/plugin-syntax-dynamic-import @babel/preset-env @fortawesome/fontawesome-free aspnet-webpack babel-loader babel-plugin-dynamic-import-webpack compression-webpack-plugin css-loader file-loader mini-css-extract-plugin node-sass rimraf sass-loader style-loader url-loader webpack webpack-cli html-webpack-plugin webpack-dev-middleware webpack-hot-middleware
5. ClientApp> npm i -S @babel/polyfill bootstrap jquery moment popper.js
* * *

## 5. Github ����
### 1) �̹� Remote git�� bin Ȥ�� obj ������ �ִ� ���
    $ git rm -r bin(obj)
    $ git commit -m "Remove bin(or obj) folder"
    $ git push origin master
* * *

## 6. Docker ����

### 0) Docker CMD
	1. ��ü ����
	$ docker ps -aq | foreach {docker stop $_}

	2. ��ü ���� 
	$ docker ps -aq | foreach {docker rm $_}

### 1) .dockerignore
    .dockerignore
    .env
    .git
    .gitignore
    .vs
    .vscode
    */bin
    */obj
    **/.toolstarget

### 2) csproj ���� �� Dockerfile ����
    # RUN ALL CONTAINERS FROM ROOT (folder with .sln file):
    # docker-compose build
    # docker-compose up
    #
    # RUN JUST THIS CONTAINER FROM ROOT (folder with .sln file):
    # docker build --pull -t web -f src/Web/Dockerfile .
    #
    # RUN COMMAND
    #  docker run --name eshopweb --rm -it -p 5106:5106 web
    FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
    WORKDIR /app

    COPY *.sln .
    COPY . .
    WORKDIR /app/src/Web
    RUN dotnet restore

    RUN dotnet publish -c Release -o out

    FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime
    WORKDIR /app
    COPY --from=build /app/src/Web/out ./

    # Optional: Set this here if not setting it from docker-compose.yml
    # ENV ASPNETCORE_ENVIRONMENT Development

    ENTRYPOINT ["dotnet", "Web.dll"]

## 7. Nuget ����
    "Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"  Version="3.0.0" 
    "Microsoft.AspNetCore.SpaServices"                   Version="3.0.0" 
    "Microsoft.AspNetCore.SpaServices.Extensions"        Version="3.0.0"
    "Swashbuckle.AspNetCore.Swagger"                     Version="5.0.0-rc4"
    "Swashbuckle.AspNetCore.SwaggerGen"                  Version="5.0.0-rc4"
    "Swashbuckle.AspNetCore.SwaggerUI"                   Version="5.0.0-rc4"

## Issue Solve
### 1) .net core 3 Runtime Update �� IIS���� "�����ڵ� ����"�� ���� ���(Asp.net Core 2.X ������Ʈ)
    1) IIS���� �� ����Ʈ ��Ŭ�� - [������Ʈ ����] - [��޼���] Ŭ��
    2) (�Ϲ�) - �������α׷� Ǯ - <Classic .NET AppPool> �� ���� �� Ȯ��

### 2) NPM package.json���� <vscode package.json String does not match the pattern> ����/��� �޽��� 
    1) package.json ���� "name"�� �ҹ��ڷθ� ǥ��(�빮�� �Է½� ��� �޽��� �߻�)

### 3) Docker desktop ���� �ȵ� ���(�����۹������� ����ǰ� ���� �����Ƿ� ���� ��ǻ�͸� �������� ���߽��ϴ�.)
    $ dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
    $ bcdedit /set hypervisorlaunchtype auto
    - CMD Windows ������ �������� ����
    - �� ��� ���� �� ��ǻ�� �����

### 4) cannot locate specified dockerfile dockerfile
    version: '3.4'

    services:
     notfoundexceptionmvc:
        image: ${DOCKER_REGISTRY-}notfoundexceptionmvc
        build:
            context: .
            dockerfile: src\Web\Dockerfile    -> src/Web/Dockerfile�� ���� 


### 5) unable to configure HTTPS endpoint. No server certificate was specified, and the default developer certificate could not be found or is out of date
	- ���� : https://docs.microsoft.com/ko-kr/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=netcore-cli#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos
	- C:\Users{USER} \AppData\Roaming\ASP.NET\Https ������ ���� �մϴ�.
	- �ַ���� ���� �մϴ�. bin �� obj ������ �����մϴ�.
	- ���� ������ �ٽ� ���� �մϴ�. ���� ��� Visual Studio, Visual Studio Code �Ǵ� Mac�� Visual Studio�Դϴ�.
