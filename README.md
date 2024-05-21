<body>
    <h1>Hydra Lab Walkthrough</h1>
    <h2>Introduction</h2>
    <p>This guide will walk you through the TryHackMe Hydra lab, teaching you how to use Hydra for brute-forcing login credentials. Ensure you have access to TryHackMe and the necessary tools installed (OpenVPN, Burp Suite, and FoxyProxy).</p>
    <h2>Prerequisites</h2>
    <ul>
        <li><strong>TryHackMe Account:</strong> Ensure you have an account on TryHackMe.</li>
        <li><strong>OpenVPN:</strong> Install and configure OpenVPN to connect to TryHackMe servers.</li>
        <li><strong>Burp Suite:</strong> Install Burp Suite for intercepting web traffic.</li>
        <li><strong>FoxyProxy:</strong> Add the FoxyProxy extension to your browser to manage proxy settings.</li>
    </ul>
    <h2>Steps</h2>
    <ol>
        <li><strong>Connect to TryHackMe</strong>
            <p>Download the VPN configuration file from TryHackMe and connect using OpenVPN:</p>
            <pre><code>sudo openvpn your-vpn-config.ovpn</code></pre>
        </li>
        <li><strong>Access the Hydra Challenge</strong>
            <p>Activate the VM within the Hydra Challenge on TryHackMe.</p>
            <p>Access the machine via the provided URL (replace <code>IPAddress</code> with the actual IP address):</p>
            <pre><code>http://IPAddress</code></pre>
        </li>
        <li><strong>Set Up Burp Suite</strong>
            <p>Open Burp Suite and navigate to the "Proxy" tab.</p>
            <p>Ensure the intercept is turned on.</p>
            <p>Configure FoxyProxy in your browser to route traffic through Burp Suite’s proxy (usually <code>127.0.0.1:8080</code>).</p>
            <img width="927" alt="Screenshot 2024-05-21 at 6 01 41 PM" src="https://github.com/Fabiany-cs/Hydra/assets/107880960/494cb8f4-397e-4ff6-b5dc-adb6ea8156b1">
        </li>
        <li><strong>Intercept Login Request</strong>
            <p>Attempt to log in on the challenge page with any credentials.</p>
            <p>Burp Suite will intercept the request. Forward the request until you see the login attempt in the HTTP history.</p>
            <p>Analyze the request to understand the structure of the username and password fields.</p>
            <img width="797" alt="Screenshot 2024-05-21 at 6 02 44 PM" src="https://github.com/Fabiany-cs/Hydra/assets/107880960/c6ff02ab-97d5-4934-ae22-a74db6aab646">
            <img width="209" alt="Screenshot 2024-05-21 at 6 04 24 PM" src="https://github.com/Fabiany-cs/Hydra/assets/107880960/e17c326e-734a-4057-b219-dffdacbc81be">
        </li>
        <li><strong>Prepare for Brute Force with Hydra</strong>
            <p>Note the structure and endpoint of the intercepted login request.</p>
            <p>Identify the form parameters for the username and password.</p>
        </li>
        <li><strong>Use Hydra for Brute Forcing</strong>
            <p>Open a terminal and use the <code>rockyou.txt</code> wordlist to brute-force the login form with Molly's username.</p>
            <p>Use the following Hydra command (replace <code>target-url</code> and <code>form-parameters</code> accordingly):</p>
            <pre><code>hydra -l molly -P /path/to/rockyou.txt target-url http-post-form "form-parameters:invalid-login-message"</code></pre>
            <img width="1171" alt="Screenshot 2024-05-21 at 6 07 00 PM" src="https://github.com/Fabiany-cs/Hydra/assets/107880960/91da9470-a782-4130-9ca8-627c8da0cda5">
            <p>Breakdown of the Hydra command:</p>
            <ul>
                <li><code>-l</code>: specifies the username (<code>molly</code>).</li>
                <li><code>-P</code>: specifies the path to the password list (<code>rockyou.txt</code>).</li>
                <li><code>target-url</code>: the URL of the login form.</li>
                <li><code>http-post-form</code>: indicates the type of request.</li>
                <li><code>form-parameters</code>: should include the username, password fields, and any additional parameters.</li>
                <li><code>invalid-login-message</code>: the message that appears on a failed login attempt (copy this from the Burp Suite intercepted response).</li>
            </ul>
        </li>
    </ol>
    <h2>Example Command</h2>
    <p>Assume the login form parameters are <code>username</code> and <code>password</code>, and the invalid login message is "Invalid credentials":</p>
    <pre><code>hydra -l molly -P /path/to/rockyou.txt http://IPAddress/login.php http-post-form "username=molly&password=^PASS^:Invalid credentials."</code></pre>
  <br>
    <h2>Actual Command in my Environment</h2>
    <pre><code>hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.136.14 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is correct."</code></pre>
    <h2>Conclusion</h2>
    <p>By following these steps, you can successfully use Hydra to brute-force login credentials in the TryHackMe Hydra lab. Remember to replace placeholder values with actual data from your lab environment.</p>
    <img width="932" alt="Screenshot 2024-05-21 at 6 16 07 PM" src="https://github.com/Fabiany-cs/Hydra/assets/107880960/9463dee7-a046-447f-b364-059405e4f973">
</body>
</html>
