# Remote Instance with SSH putty
 1. Pastikan sudah install putty
![Alt text](image-12.png)

 2. Konversi file Public Key dari .pem menjadi .ppk di putty
 - buka puttyGen
 - load File .pem
 - Save as .ppk
 ![Alt text](image-13.png)

 3. Set Up Putty untuk Remote SSH
- buka apps Putty
- Isi IP Public 
- Isi port untuk SSH sesuai Security Group di Instance
- Isi Nama session agar saat connect lagi tinggal load aja
- load file .ppk (Klik SSH -> Auth -> Credentials)
![Alt text](image-14.png)
![Alt text](image-15.png)
![Alt text](image-16.png)

4. Sudo apt-get Update (Update OS) lanjut sudo apt-get Upgrade
![Alt text](image-17.png)

5. Pembuktian Remote SSH secara visual
- Copy public IP Address instance paste ke Browser
![Alt text](image-18.png)
-Install Web Server seperti Apache/Nginx
-sudo apt install apache2
-Reload Browser 
![Alt text](image-19.png)

6. Matikan Instance agar tidak kena tagihan
- sudo shutdown now
![Alt text](image-20.png)