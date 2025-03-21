Installing Jenkins in Linux and creating a freestyle Nginx project
sudo apt install -y openjdk-17-jdk
sudo update-alternatives --config java ( Installing JDK17) .
![Screenshot from 2025-03-20 09-53-30](https://github.com/user-attachments/assets/28e33645-ab72-46bf-b898-7a9b0b761c23)
After this you'll be prompted to select a java version select the index with java-17
![Screenshot from 2025-03-20 09-53-44](https://github.com/user-attachments/assets/708fea59-e8d2-4875-b70e-30965409868c)
Installing Jenkins..
![Screenshot from 2025-03-20 09-53-51](https://github.com/user-attachments/assets/3e869e56-814a-4baa-a2d6-a25dc9f01b8f)
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
![Screenshot from 2025-03-20 09-54-00](https://github.com/user-attachments/assets/6335bc1b-1c6e-4bce-8bbf-03dae051de6f)
Starting Jenkins..
![Screenshot from 2025-03-20 09-59-37](https://github.com/user-attachments/assets/105d3e2b-c527-49f8-baae-b71d05bbf468)
sudo systemctl status jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
![Screenshot from 2025-03-20 09-59-45](https://github.com/user-attachments/assets/f858773e-0c63-4a33-a4d6-60a99e1ab549)

