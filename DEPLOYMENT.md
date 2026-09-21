# CV Website Deployment

Replace the placeholder name, student ID, contact details, education, and GitHub URL in `index.html` before publishing.

## Push to GitHub

```bash
git init
git add index.html styles.css DEPLOYMENT.md
git commit -m "Create CV profile website"
git branch -M main
git remote add origin https://github.com/Kishhh19/Deploying-a-Static-Website-in-Apache-Web-Server.git
git push -u origin main
```

## Prepare Amazon Linux EC2

For the current instance shown in the AWS console:

- Public IP: `15.135.234.242`
- Website URL after deployment: `http://15.135.234.242`

From PowerShell on your workstation, connect with the key file assigned when the instance was created:

```powershell
ssh -i "C:\path\to\your-key.pem" ec2-user@15.135.234.242
```

If SSH reports a timeout, allow inbound SSH (TCP port 22) from your current IP in the instance security group. If the website later reports a timeout, also allow inbound HTTP (TCP port 80) from `0.0.0.0/0`.

Connect with the SSH command from the EC2 console, then run:

```bash
sudo dnf update -y
sudo dnf install -y httpd git
sudo systemctl start httpd
sudo systemctl enable httpd
```

For older Amazon Linux images, use `yum` instead of `dnf`.

## Deploy the website

```bash
cd /tmp
git clone https://github.com/Kishhh19/Deploying-a-Static-Website-in-Apache-Web-Server.git
sudo rm -rf /var/www/html/*
sudo cp -r Deploying-a-Static-Website-in-Apache-Web-Server/* /var/www/html/
sudo chown -R apache:apache /var/www/html
sudo systemctl restart httpd
```

In the EC2 security group, add an inbound rule for **HTTP**, protocol **TCP**, port **80**, source `0.0.0.0/0`. Then open `http://EC2-PUBLIC-IP` in a browser.

## Submission checklist

- Replace all placeholders in the website.
- Capture a full-page screenshot showing the deployed page.
- Record your name, student ID, and EC2 public IP.
- Add the screenshot and the commands above to the Google Doc.
- Show the result to the instructor, then terminate the EC2 instance.
