### AWS

- Log into the AWS Console.
- [AWS Login](https://aws.amazon.com/console/)
- Go to EC2 and Launch Instance
- Select Amazon Linux 2 AMI (Free Tier Eligible)
  - Select t2.micro
  - Review and Launch Instance
  - Launch Instance\*
  - Create a new Key Pair
  - Be sure to download PEM and save file
  - Launch Instance
  - Edit Security Group for Inbound Data
    - Custom TCP 8000 Anywhere 0.0.0.0/0

### SSH

- Navigate to .ssh
  - copy pem file to .ssh
  - run chmod 400 on file.
- Obtain the Ec2 Instance Public IP
- EC2->Instance->check Box -> Connect ->SSH Client
- ssh -i "somf-file.pem" ec2-user@ec2-54-203-8-100.us-west-2.compute.amazonaws.com

## EC2 Instance

- See updates needed sudo yum update
- sudo yum install git
- clone repo (Be sure to select HTTPS)
- sudo yum install -y docker
- sudo usermod -a -G docker ec2-user
- sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose
- sudo service docker start
- sudo chkconfig docker on
- sudo rm /etc/localtime
- sudo docker swarm init

- sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
- sudo reboot
- Add missing .env
- Update allowed_hosts with EC2 IP
- Change Debug to False if not already done.

Test the Public IP on port 8000

### Heroku deployemnt guide

Deploy Project - Heroku Time
install heroku cli
heroku apps:create snacks-api
Check remote with git remote -v
If heroku remote doesn't show then heroku git:remote -a snacks-api
Create heroku.yml in root folder.
Add below text to heroku.yml
build:
  docker:
    web: Dockerfile
release:
  image: web
  command:
    - python manage.py collectstatic --noinput
run:
  web: gunicorn project.wsgi
EXTRA IMPORTANT STEP
heroku stack:set container
Add/Commit
git push heroku main
Go to heroku
Login takes you to dashboard
Select your app
Go to settings
Click reveal config vars button
Add config vars to match .env file
ALLOWED_HOSTS should match the heroku URL for your app.
Click Open app button to see it
Leave out the https:// and trailing slash.
E.g. snacks-api.herokuapp.com
It can take a minute for the environment variable changes to take effect
Once site is ready then see if you can log in, create snacks, etc.
It will be ugly because the styling was lost.
This is due to the Heroku file system's "ephemeral" nature
One way to handle issue is to run collectstatic locally then commit the staticfiles to heroku.

### Collaboration

I use the class repo and collaborated with Daniel Dills, Davee Sok, and Prabin Singh.
Special appreciation for Michael Herndriks who helped me deploying my app on heroku