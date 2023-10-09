# Creating a VPC

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

## Step 5 (Connecting the Route to our Internet Gateway)

From our Route Tables, click 'Edit Route'

![Screenshot 2023-10-09 174741.png](images%2FScreenshot%202023-10-09%20174741.png)

We want to add another route so click 'Add Route', we want to allow all I.P.s so we select 0.0.0.0/0 for destination

![Screenshot 2023-10-09 174838.png](images%2FScreenshot%202023-10-09%20174838.png)
- For the Target, we want to select Internet Gateway and then select our VPC

Click 'Save Changes'













