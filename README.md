# end-of-semester
Web Application Prototype Deployment (AWS)
This repository contains the necessary files and detailed steps for deploying a dynamic web application prototype on an Ubuntu server using Amazon Web Services (AWS) EC2, utilizing Nginx as a reverse proxy for a Node.js application, and securing it with Let's Encrypt SSL.

You can access the live prototype at the following public IP address:
Public IP Address: http://44.209.231.154/
Server Provisioning (AWS EC2)
This section details the steps to provision an Ubuntu server on AWS EC2.
Log in to AWS Console: Go to the AWS Management Console and log in to your account.
Navigate to EC2: In the search bar, type "EC2" and select it from the services.
Launch Instance:
Click on "Launch Instances".
Name: Give your instance a descriptive name (e.g., web-app-prototype).
Application and OS Images (Amazon Machine Image - AMI): Choose "Ubuntu Server 22.04 LTS (HVM), SSD Volume Type" (or the latest LTS version available). Ensure it's a "Free tier eligible" option if you're using a new account.
Instance Type: Select t2.micro (Free tier eligible).
Key pair (login):
Choose an existing key pair or create a new one. If creating a new one, download the .pem file immediately and keep it secure. This file is crucial for SSH access.
Network settings:
Firewall (Security groups):
Create a new security group.
Add inbound rules to allow SSH (Port 22) from "My IP" or "Anywhere" (be cautious with Anywhere).
Add inbound rules to allow HTTP (Port 80) from "Anywhere".
Add inbound rules to allow HTTPS (Port 443) from "Anywhere".
Configure storage: Default 8 GiB gp2 SSD is usually sufficient for a prototype.
Review and Launch: Review your configuration and click "Launch instance".
Connect to your instance: Once the instance state changes to "Running", select your instance, click "Connect", and follow the instructions to SSH into your server.
For Linux/macOS users (using Terminal):
chmod 400 /path/to/your-key-pair.pem
ssh -i /path/to/your-key-pair.pem ubuntu@http://44.209.231.154/

For Windows users (using PuTTY):
Convert your .pem key to .ppk using PuTTYgen:
Download PuTTY and PuTTYgen from the official PuTTY website.
Open puttygen.exe.
Click "Load" and select your .pem file (you might need to select "All Files (*.*)" in the file type dropdown).
Click "Save private key" and save the generated .ppk file (e.g., my-key-pair.ppk).
Connect using PuTTY:
Open putty.exe.
In the "Session" category, enter ubuntu@YOUR_PUBLIC_IP_ADDRESS in the "Host Name (or IP address)" field.
Navigate to Connection > SSH > Auth in the left-hand category tree.
Click "Browse..." and select your saved .ppk file.
Go back to the "Session" category, enter a name under "Saved Sessions" (e.g., "My EC2 Instance"), and click "Save".
Click "Open" to start the SSH session.
Web Server Setup (Nginx & Node.js)
This section covers installing Nginx and Node.js, and configuring Nginx as a reverse proxy for your Node.js application.
1. Update and Upgrade Packages
First, update your server's package list:
sudo apt update
sudo apt upgrade -y


2. Install Node.js and npm
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs


Verify installation:
node -v
npm -v


3. Install Nginx
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx


Verify Nginx is running:
sudo systemctl status nginx


You should see active (running).
4. Install PM2 (Process Manager for Node.js)
PM2 is essential for keeping your Node.js application running in the background and restarting it on server reboots.
sudo npm install -g pm2


Dynamic Landing Page
The landing page is a simple HTML file (index.html) styled with Tailwind CSS, and a Node.js application (app.js) to serve it.
1. Create Project Directory
mkdir ~/web-app-prototype
cd ~/web-app-prototype


2. Create index.html
Create an index.html file in the ~/web-app-prototype directory with your personalized content.
    @keyframes slideInLeft {
        to { opacity: 1; transform: translateX(0); }
    }
    @keyframes slideInRight {
        to { opacity: 1; transform: translateX(0); }
    }
    @keyframes fadeIn {
        to { opacity: 1; }
    }
</style>


    <!-- Header Section -->
    <header class="text-center lg:text-left lg:w-1/3 flex flex-col justify-center items-center lg:items-start p-6 bg-gradient-to-br from-blue-500 to-cyan-500 text-white rounded-lg shadow-lg pulsate">
        <h1 class="text-4xl md:text-5xl font-extrabold mb-4 slide-in-left">
            The Future of AI-Powered Logistics
        </h1>
        <p class="text-xl md:text-2xl font-light mb-6 slide-in-right">
            A Prototype by **[Your Name]**, Lead Cloud Engineer
        </p>
        <div class="w-24 h-24 bg-white rounded-full flex items-center justify-center mb-4 shadow-xl">
            <!-- Placeholder for a simple icon or image -->
            <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 text-blue-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 17L9 20l-1 1h4c0 0 0 0 0 0h4l-1-1-0.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
            </svg>
        </div>
    </header>

    <!-- Main Content Section -->
    <main class="lg:w-2/3 p-6 space-y-8">
        <!-- Project Pitch -->
        <section class="bg-blue-50 p-6 rounded-lg shadow-md fade-in">
            <h2 class="text-3xl font-semibold text-blue-800 mb-4">Project Pitch</h2>
            <p class="text-gray-700 text-lg leading-relaxed">
                Our AI-Powered Logistics platform revolutionizes supply chain management by optimizing routes, predicting delays, and automating inventory. This innovative solution leverages machine learning to reduce operational costs and significantly improve delivery efficiency, setting a new standard for precision in logistics.
            </p>
        </section>

        <!-- Professional Bio -->
        <section class="bg-gray-50 p-6 rounded-lg shadow-md fade-in">
            <h2 class="text-3xl font-semibold text-gray-800 mb-4">Professional Bio</h2>
            <div class="text-gray-700 text-lg space-y-4">
                <p>
                    As a **Lead Cloud Engineer**, I specialize in designing, deploying, and managing scalable cloud infrastructures on AWS, Azure, and Google Cloud. My expertise spans across containerization (Docker, Kubernetes), CI/CD pipelines, and serverless architectures.
                </p>
                <p>
                    **Skills:** AWS EC2, S3, Lambda, RDS; Azure VMs, App Services; Google Cloud Compute Engine; Docker; Kubernetes; Terraform; Ansible; Nginx; Apache; Node.js; Python; Bash Scripting; CI/CD; System Monitoring.
                </p>
                <p>
                    **Past Projects:**
                    <ul class="list-disc list-inside ml-4">
                        <li>Developed a highly available e-commerce platform backend on AWS.</li>
                        <li>Implemented automated deployment pipelines for a multi-service SaaS application.</li>
                        <li>Migrated legacy applications to containerized environments.</li>
                    </ul>
                </p>
                <p>
                    **Education:** M.Sc. Computer Science, University of [Your University Name]; B.Eng. Software Engineering, University of [Your Other University Name].
                </p>
            </div>
        </section>
    </main>

</div>


3. Create app.js (Node.js Application)
Create an app.js file in the ~/web-app-prototype directory. This simple Node.js script will serve the index.html file.
// Define the port the server will listen on
const port = 3000;
// Create an HTTP server
const server = http.createServer((req, res) => {
// Log the incoming request URL for debugging purposes
console.log(Request received for: ${req.url});
// Determine the file path for the requested resource
// path.join safely constructs a path, preventing directory traversal issues
const filePath = path.join(__dirname, 'index.html');

// Read the index.html file asynchronously
fs.readFile(filePath, (err, data) => {
    // If an error occurs during file reading (e.g., file not found)
    if (err) {
        console.error(`Error reading file ${filePath}:`, err);
        // Set the HTTP status code to 500 (Internal Server Error)
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        // Send an error message to the client
        res.end('Internal Server Error: Could not load page.');
        return; // Exit the function
    }

    // If the file is read successfully
    // Set the HTTP status code to 200 (OK)
    // Set the Content-Type header to text/html to tell the browser it's an HTML document
    res.writeHead(200, { 'Content-Type': 'text/html' });
    // Send the content of the HTML file as the response body
    res.end(data);
});


});
// Start the server and listen on the defined port
server.listen(port, () => {
// Log a message to the console once the server is successfully running
console.log(Server running at http://localhost:${port}/);
console.log('Serving index.html');
});
</pre>
</details>

### 4. Start Node.js Application with PM2

From within the `~/web-app-prototype` directory:

```bash
pm2 start app.js --name "landing-page-app"
pm2 save
pm2 startup
```pm2 startup` will generate a command you need to run to configure PM2 to start your application automatically on boot. Copy and paste that command into your terminal and execute it.

You can check the status of your app:

```bash
pm2 status


Networking & Security
This section covers securing your web application.
1. AWS Security Group Configuration
Ensure your EC2 instance's security group allows inbound traffic on ports 80 (HTTP) and 443 (HTTPS). You should have configured this during instance provisioning, but double-check:
Go to your EC2 Dashboard -> Security Groups.
Select the security group associated with your instance.
Under "Inbound rules", ensure you have rules allowing:
Type: HTTP, Protocol: TCP, Port Range: 80, Source: 0.0.0.0/0 (or your specific IP)
Type: HTTPS, Protocol: TCP, Port Range: 443, Source: 0.0.0.0/0 (or your specific IP)
2. Configure Nginx as a Reverse Proxy
Create an Nginx configuration file for your application. This tells Nginx to listen on port 80 and forward requests to your Node.js app (running on port 3000).
sudo nano /etc/nginx/sites-available/web-app-prototype


Paste the following configuration into the file:
# Replace with your public IP or custom domain name (e.g., your_domain.com www.your_domain.com)
# If using a public IP, use it here. If using a custom domain, specify it.
server_name YOUR_PUBLIC_IP_ADDRESS_OR_DOMAIN;

# Root directory for Nginx, though requests will be proxied.
# This is a fallback and can be omitted if all traffic is proxied.
root /var/www/html;
index index.html index.htm;

# This location block handles all incoming requests.
location / {
    # Proxy requests to the Node.js application running on localhost:3000.
    proxy_pass http://localhost:3000;

    # These headers are important for the Node.js application to correctly
    # identify the client's IP address and the original host.
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # Disable buffering to send data to the client as it's received from the backend.
    proxy_buffering off;
}

# Certbot will automatically add an HTTPS server block here after you run it,
# similar to this (example, actual block will be generated by Certbot):
#
# listen 443 ssl; # managed by Certbot
# ssl_certificate /etc/letsencrypt/live/YOUR_PUBLIC_IP_ADDRESS_OR_DOMAIN/fullchain.pem; # managed by Certbot
# ssl_certificate_key /etc/letsencrypt/live/YOUR_PUBLIC_IP_ADDRESS_OR_DOMAIN/privkey.pem; # managed by Certbot
# include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
# ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

# If you later set up a custom domain and SSL, Certbot will create a new server block
# listening on port 443 and automatically add the necessary SSL directives.
# It will also likely add a redirect from HTTP to HTTPS in this block or a separate one.


}
</pre>
</details>

Save and exit (`Ctrl+X`, `Y`, `Enter`).

### 3. Enable Nginx Configuration

Create a symbolic link to enable the configuration:

```bash
sudo ln -s /etc/nginx/sites-available/web-app-prototype /etc/nginx/sites-enabled/


Test Nginx configuration for syntax errors:
sudo nginx -t


If the test is successful, restart Nginx to apply changes:
sudo systemctl restart nginx


4. Secure with Let's Encrypt SSL (Certbot)
To enable HTTPS, use Certbot to obtain and install a free SSL certificate from Let's Encrypt.
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot


Now, run Certbot to obtain the certificate. Replace your_domain.com with your actual public IP address or domain name if you have one configured (e.g., ec2-XX-XXX-XXX-XXX.compute-1.amazonaws.com).
sudo certbot --nginx -d YOUR_PUBLIC_IP_ADDRESS_OR_DOMAIN


Follow the prompts. Certbot will automatically modify your Nginx configuration to include the SSL certificate and set up automatic renewal.
Note: If you are using a public IP address directly (not a registered domain name), Certbot might have issues issuing certificates. It's best practice to use a custom domain name for SSL. For a prototype, accessing via http://YOUR_PUBLIC_IP_ADDRESS might suffice initially before setting up a domain and SSL.
Deployment Steps
Provision EC2 Instance: Follow the steps in Server Provisioning (AWS EC2).
SSH into Instance: Connect to your EC2 instance using the provided SSH command.
Update and Install Dependencies: Execute the commands in Update and Upgrade Packages, Install Node.js and npm, and Install Nginx.
Install PM2: Follow steps in Install PM2 (Process Manager for Node.js).
Create Project Directory: Follow steps in Create Project Directory.
Create index.html and app.js: Copy the content provided by the AI into index.html and app.js files within your ~/web-app-prototype directory.
Start Node.js App with PM2: Follow steps in Start Node.js Application with PM2.
Configure Nginx: Follow steps in Configure Nginx as a Reverse Proxy and Enable Nginx Configuration.
Secure with SSL (Optional but Recommended): Follow steps in Secure with Let's Encrypt SSL (Certbot).
Verify Deployment: Open your browser and navigate to your public IP address or custom domain.

