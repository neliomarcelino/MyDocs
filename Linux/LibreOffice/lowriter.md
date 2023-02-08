## Convert document to PDF

>lowriter is a part of LibreOffice

For example, convert all `.docx` files onto `.pdf`
```bash
lowriter --convert-to pdf *.docx
```

---
>[!Note]
> If executing the command above returns the error:
>  `failed to launch javaldx - java may not function correctly`
>  
>  Run the following command:
> `sudo apt-get install default-jre libreoffice-java-common`

