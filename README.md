# b3networks-technical-test

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
