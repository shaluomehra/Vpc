# Creating a VPC
![Untitled Diagram.drawio.png](images%2FUntitled%20Diagram.drawio.png)
### Step 1 (Creating the Initial VPC)

Navigate to the VPC page, using the search bar at the top of the AWS console and then click 'Create VPC' <br>
Using our naming conventions we want to fill out the necessary spaces

![Screenshot 2023-10-09 170153.png](images%2FScreenshot%202023-10-09%20170153.png)

- We only need IPv4 
- 10.0.0.0/16 only allows us to play with the last two parts of the i.p. address

### Step 2 (Creating the Subnets)

Navigate using the left pane to 'Subnet' and then click 'Create a subnet', for the VPC ID we need to find our initial VPC that we created

![Screenshot 2023-10-09 171556.png](images%2FScreenshot%202023-10-09%20171556.png)
- Choose an appropriate subnet name usually (public or private)
- We want to select an availability zone because when we create the private subnet we want to ake sure it's in a different availability zone

![Screenshot 2023-10-09 172142.png](images%2FScreenshot%202023-10-09%20172142.png)
- We change the IPv4 subnet CIDR Block to `10.0.2.0/24` as we only want to work with the last part of the i.p

Now we want to add our private subnet, so click 'Add new subnet'

![Screenshot 2023-10-09 172551.png](images%2FScreenshot%202023-10-09%20172551.png)
- This time we want to use availability zone 1b
- Change the IPv4 subnet CIDR Block to `10.0.3.0/24` making sure it's different from the public

Click 'Create Subnet'

### Step 3 (Create our Internet Gateway)

Navigate using the left pane, to 'Internet Gateway' and click 'Create Internet Gateway'

![Screenshot 2023-10-09 173246.png](images%2FScreenshot%202023-10-09%20173246.png)
- This step is pretty simple, write an appropriate name
- The tags should be filled out for you

Click 'Create internet gateway' <br>

Next you need to attach the Internet gateway to your VPC so click 'Actions' and then click 'Attach to VPC'

![Screenshot 2023-10-09 173401.png](images%2FScreenshot%202023-10-09%20173401.png)

You'll be taken to this screen, find your VPC and attach it

![Screenshot 2023-10-09 173510.png](images%2FScreenshot%202023-10-09%20173510.png)

### Step 4 (Creating our Route Table)

We need to create a route table for our public subnet since our private one will be created automatically

![Screenshot 2023-10-09 173850.png](images%2FScreenshot%202023-10-09%20173850.png)
- Make sure you select your VPC

Click 'Create route table'

Next we want to connect our route table to our Public Subnet, from this page click 'Subnet associations' then click 'Edit subnet associations'

![Screenshot 2023-10-09 174105.png](images%2FScreenshot%202023-10-09%20174105.png)

![Screenshot 2023-10-09 174352.png](images%2FScreenshot%202023-10-09%20174352.png)
- Click on the public subnet
- Click Save association

### Step 5 (Connecting the Route to our Internet Gateway)

From our Route Tables, click 'Edit Route'

![Screenshot 2023-10-09 174741.png](images%2FScreenshot%202023-10-09%20174741.png)

We want to add another route so click 'Add Route', we want to allow all I.P.s so we select 0.0.0.0/0 for destination

![Screenshot 2023-10-09 174838.png](images%2FScreenshot%202023-10-09%20174838.png)
- For the Target, we want to select Internet Gateway and then select our VPC

Click 'Save Changes'

### Step 6 (Starting our Database Instance)

First we are going to start our private subnet (which is our database instance) <br>

We load up our image with our database on it which we created earlier, to do this find the image on AMI's and 'Launch instance from AMI'

![Screenshot 2023-10-10 092633.png](images%2FScreenshot%202023-10-10%20092633.png)
- Give an appropriate name
- AMI should already be selected

Fill out the rest as usual and now we want to create a new security group **under our new VPC**

![Screenshot 2023-10-10 092826.png](images%2FScreenshot%202023-10-10%20092826.png)
- Select your VPC
- Change the subnet to the private subnet
- We want to Disable 'Auto-assign public IP' since we don't need it
- Create a new security group allowing only the SSH and Mongo ports
![Screenshot 2023-10-10 093520.png](images%2FScreenshot%202023-10-10%20093520.png)

Once double checking everything, click 'Launch Instance'

### Step 7 (Starting our APP Instance)

Now we need to start our public subnet (which is our app instance)

'Launch the Instance' using your app AMI

![Screenshot 2023-10-10 093912.png](images%2FScreenshot%202023-10-10%20093912.png)
- Give an appropriate name
- AMI should already be selected

Fill out the rest as usual and now we want to create a new security group **under our new VPC**

![Screenshot 2023-10-10 094144.png](images%2FScreenshot%202023-10-10%20094144.png)
- Select your VPC
- Change the subnet to the public subnet
- We want to **Enable** 'Auto-assign public IP' since we want anyone to join the website
- Create a new security group allowing only the SSH, HTTP and 3000 ports
![Screenshot 2023-10-10 094538.png](images%2FScreenshot%202023-10-10%20094538.png)

Next we want to fill in the user details so we can run the app and the database

Scroll down to the 'Additional Details' tab and expand it and scroll to the user details

```
#!/bin/bash

export DB_HOST=mongodb://<private-ip-for-database>/posts

cd /home/ubuntu/repo/app
npm install

node seeds/seed.js

sudo systemctl restartnginx

pm2 start app.js
```

Copy and paste this into the user details, to find the Private IP number of the database, go to the instance details and you can see it there.
![Screenshot 2023-10-10 095209.png](images%2FScreenshot%202023-10-10%20095209.png)

Once you check everything, click 'Launch Instance'

### Step 8 (Load up the website on the Web Browser)

Paste in the app public i.p. address into a web browser with /posts to check if everything is running smoothly

![Screenshot 2023-10-10 095518.png](images%2FScreenshot%202023-10-10%20095518.png)




















