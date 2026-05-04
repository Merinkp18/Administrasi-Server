# Modernisasi CI/CD (Continous Integration/Contious Delivery)

1. Mengisi Secrets Variable di Github Actions
    - Buka repository di Github
    - Klik settings -> secrets and variables -> Action
    - Klik new repository secret
    - Isi nama = DOCKERHUB_USERNAME dan value = merinkharista
    - Klik new repository secret
    - Isi nama = DOCKERHUB_TOKEN dan value = token akun dockerhub
    - Klik new repository secret
    - Isi nama = AWS_HOST dan value = ip address EC2 instance
    - Klik new repository secret
    - Isi nama = AWS_USERNAME dan value = ubuntu
    - Klik new repository secret
    - Isi nama = AWS_PRIVATE_KEY dan value = file .pem (berisi tanda peitk awal dan akhir juga)
![Alt text](image.png)

2. Melakukan Edit File Pipeline di Github
    - Buka projek compro_2388010050
    - Buat folde rbaru .github -> Buat folder workflows -> buat file deploy.yaml
    - Isi file deploy.yaml sebagai berikut 

name: Deploy Web Statis to AWS EC2
on:
  push:
    branches: [ main ]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Login to Docker Hub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKERHUB_USERNAME }}/compro_2388010050:latest

  deploy:
    needs: build-and-deploy
    runs-on: ubuntu-latest
    name: Deploy to EC2 via SSH and run docker compose
    steps:
    - name: SSH and deploy
      uses: appleboy/ssh-action@v1.0.3
      with:
        host: ${{ secrets.AWS_HOST }}
        username: ${{ secrets.AWS_USERNAME }}
        key: ${{ secrets.AWS_PRIVATE_KEY }}
        port: 22
        script: |
          docker rm -f compro_2388010050 || true
          docker pull ${{ secrets.DOCKERHUB_USERNAME }}/compro_2388010050:latest
          docker run -d --name compro_2388010050 -p 80:80 ${{ secrets.DOCKERHUB_USERNAME }}/compro_2388010050:latest

![Alt text](image-1.png)

3. Sebelum melakukan commit dan Synch pada File
    - Pastikan sudah disable apache2 -> sudo systemctl disable apache2
    - Pastikan sudah stop apache2 -> sudo systemctl stop apache2
    - Baru lakukan Commit dan Push ke Github
![Alt text](image-2.png)