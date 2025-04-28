請遵循以下流程檢查並設定開發環境。

please do git commit for each step when you run command of edit files. If nothing to commit, just ignore it.

1. Make sure git command line tool has been installed.

    ```ps1
    git --version
    ```

    If there is no Git CLI been installed, please stop execution.

2. Make sure the working tree is clean.

    ```ps1
    git status
    ```

    If the working directory is not clean, please stop execution.

3. Check for .NET SDK version. It must be larger than `9.0.100`.

    ```ps1
    dotnet --version
    ```

4. Use `dotnet` CLI to create project by using `webapp` template without assign `-n` argument. This will create an ASP.NET Core project with Razor Pages.

    ```ps1
    dotnet new webapp
    ```

5. Setup C# Global Usings in the project.

    The filename must be `GlobalUsings.cs`. Add some common namespace to it.

6. Add `.gitignore` file

    ```ps1
    dotnet new gitignore
    ```

7. Add `.gitattributes` file that avoid cross-platform issues.

    Here is the default template for `.gitattributes`. Add the content to the `.gitattributes` file.

    ```txt
    # Set default behavior to automatically normalize line endings.
    * text=auto

    # Force batch scripts to always use CRLF line endings so that if a repo is accessed
    # in Windows via a file share from Linux, the scripts will work.
    *.{cmd,[cC][mM][dD]} text eol=crlf
    *.{bat,[bB][aA][tT]} text eol=crlf

    # Force bash scripts to always use LF line endings so that if a repo is accessed
    # in Unix via a file share from Windows, the scripts will work.
    *.sh text eol=lf
    
    .env text eol=lf
    Dockerfile text eol=lf

    # Denote all files that are truly binary and should not be modified.
    *.mp3 binary
    *.wav binary
    *.bmp binary
    *.png binary
    *.jpg binary
    *.gif binary
    ```

    > Reference: [.gitattributes Best Practices](https://rehansaeed.com/gitattributes-best-practices/)

8. Add `.editorconfig` file

    ```ps1
    dotnet new editorconfig
    ```

9. Modify `.editorconfig` to enforce coding style.

    Please merge these `.editorconfig` settings to the `.editorconfig` file:

    ```txt
    [*]
    insert_final_newline = true
    
    # Set default charset for utf-8-bom
    [*.{cs,csx,vb,vbx,txt,html,htm,css,js,json,yml,yaml,xml,config,ini,sh,ps1,psm1,psd1,ps1xml,psrc1xml,csproj,sln,gitignore,gitattributes,editorconfig,md,markdown,txt,asciidoc,adoc,asc,asciidoc,txt,ipynb,py}]
    charset = utf-8-bom
    ```

Let's do this step by step.
