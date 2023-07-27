# bitbucket-pipeline-test

## Mempersiapkan akun Bitbucket

Ada beberapa hal yang perlu dipersiapkan untuk melakukan cicd melalui bitbucket pipelines

Yang pertama wajib untuk mengaktifkan `Two-step verification` bisa diakses melalui website dibawah ini
```
https://bitbucket.org/account/settings/two-step-verification/manage
```

Jika sudah, selanjutnya aktifkan pipelines dengan mengikuti beberapa langkah berikut

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/620b7986-db13-4116-afa7-e5982a42a1e6)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/5a4300a0-e760-448d-8c40-c943a1142c56)


## Menambahkan ssh public key 

Sebelum membuat repository tidak lupa untuk menambahkan ssh public key (local) yang sudah terbuat dengan mengikuti beberapa langkah berikut

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/e099d103-a573-4692-83b1-78b02fd24b8c)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/05c5ea60-722c-4e58-875d-19f354f5fead)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/ce1aaabd-dec5-45c2-8d34-bf91e30cecbd)


## Membuat Repository pada Bitbucket

Baik selanjutnya saya membuat repository untuk aplikasi frontend dumbmerch

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/fb668d10-2899-41fa-ba2d-3d7932e42ea1)

Adapun saya membuat file baru seperti 
- bitbucket-pipelines.yml = script untuk cicd dengan bitbucket pipelines
- Dockerfile = file config untuk kebutuhan docker image build (containerization)

## Repository variables & SSH Key Pair

Sebelum melakukan pipeline ada beberapa credentials yang perlu saya buat pada Repository variables seperti :
- USER = user server
- SERVER = ip server
- SSH_KEY = private key yang sudah di encode
- PASS_DOCKER = password docker account

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/a6d134ee-c3cf-42ba-a6ca-b37cb6f64245)

Dan juga membuat ssh key pair yang akan saya tambahkan pada authorized_keys pada server sebagai izin pengenal

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/db76011d-800f-43f1-b643-253413f37535)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/6dba0684-96d5-4f95-9926-7ccd6f812116)

## Bitbucket Pipelines

Berikut proses saya melakukan CI/CD dengan Bitbucket Pipelines, adapun step yang saya lakukan yaitu :
- Check Server Connection  
- Build & Push Docker
- Install Docker on Server
- Running App on top Docker (React App)

`bitbucket-pipelines.yml`

```
pipelines:
    default:
      - step:
          name: Check Server Connection
          script:
            - pipe: atlassian/ssh-run:0.4.2
              variables:
                SSH_USER: $USER
                SERVER: $SERVER
                COMMAND: 'echo Success'
                SSH_KEY: $SSH_KEY
      - step:
          name: Build & Push Docker
          image: docker/compose:1.29.2
          services:
            - docker
          caches:
            - docker
          script:
            - docker build . --file Dockerfile --tag nikymn/dumbmerch:latest
            - echo ${PASS_DOCKER} | docker login --username "nikymn" --password-stdin
            - docker push nikymn/dumbmerch:latest
      - step:
          name: Install Docker on Server
          script:
            - pipe: atlassian/ssh-run:0.4.2
              variables:
                SSH_USER: $USER
                SERVER: $SERVER
                SSH_KEY: $SSH_KEY
                COMMAND: |
                  curl -fsSL https://get.docker.com -o get-docker.sh
                  sudo sh get-docker.sh
                  sudo usermod -aG docker $USER
      - step:
          name: Running App on top Docker
          script:
            - pipe: atlassian/ssh-run:0.4.2
              variables:
                SSH_USER: $USER
                SERVER: $SERVER
                SSH_KEY: $SSH_KEY
                COMMAND: "docker run -d -p 3000:3000 nikymn/dumbmerch:latest"
 ```
 
`Dockerfile`
 
```
FROM node:16-slim
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "npm", "start" ]
```

## Result

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/9e427599-813e-41b7-9962-52a3df3e3dec)

[18.140.54.231](http://18.140.54.231:3000)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/6a992ac9-ee23-4511-97b9-a701c90d5d5d)

[nikymn/dumbmerch:latest](https://hub.docker.com/r/nikymn/dumbmerch)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/3615e0a7-8628-40da-bcdd-b99d63aa00a0)

![image](https://github.com/mnafies/b3networks-technical-test/assets/52950376/e5f2cbc5-f009-4b0a-aaa8-c405892e9efd)

