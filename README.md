# nginx-docker-ssl

# Objetivo
Criar certificado auto-assinado e configurar um servidor HTTPS com nginx como proxy reverso de uma aplicação.

# Observação
Utilize o wsl/wsl2 ou um terminal bash. Caso terminal bash seja no windows utilize antes do openssl o ```winpty```. 

Exemplo: ```winpty openssl ...```

# 0 - Como rodar ?
Instale o certificado e execute ```docker-compose up --build``` ou execute os próximos passos para criar um novo certificado e rodar a aplicação.

# 1 - Crie/edite o config do certificado dentro da pasta ```config```
Exemplo:

```
[req]
default_bits       = 2048
default_keyfile    = localhost.key
distinguished_name = req_distinguished_name
req_extensions     = req_ext
x509_extensions    = v3_ca

[req_distinguished_name]
countryName                 = Brazil
countryName_default         = BR
stateOrProvinceName         = São Paulo
stateOrProvinceName_default = São Paulo
localityName                = Santa Rosa de Viterbo
localityName_default        = Santa Rosa de Viterbo
organizationName            = phillrog

[req_ext]
subjectAltName = @alt_names

[v3_ca]
subjectAltName = @alt_names

[alt_names]
DNS.1   = localhost
DNS.2   = 127.0.0.1
```


# 2 - Criar o certificado auto-assinado e a chave
Informe uma senha para a chave e execute o comando abaixo passando a senha no lugar de "SENHA_AQUI"

```openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout site.key -out site.crt -config ../config/site.conf -passin pass:SENHA_AQUI```

# 3 - Criar o pfx
Execute o comando abaixo para criar o arquivo pfx:

```openssl pkcs12 -export -out site.pfx -inkey site.key -in site.crt```

Será necessário informar a senha utilizada anteriormente.

# 4 - Instalar certificado
Instale o certificado criado anteriormente na máquina.

# 5 - Rodar a aplicação
Rode a aplicação executando o seguinte comando.

```docker-compose up --build```

# Resultado Final

![image](https://user-images.githubusercontent.com/8622005/178603561-db15c314-cd25-4912-91c5-44c77b091565.png)


# Referências

* [openssl req](https://www.openssl.org/docs/man1.0.2/man1/openssl-req.html)
* [winpty](https://stackoverflow.com/questions/34156938/openssl-hangs-during-pkcs12-export-with-loading-screen-into-random-state)
* [nginx](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/)
