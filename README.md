<h2>***Disclaimer: For ethical purposes only, you need consent from system owners to do this activity on their machine. This demonstration was done in a home lab environment.***</h2>

<h1>Demonstration on cracking a Linux hashed password using Kali Linux with John the Ripper</h1>

<h2>Description</h2>
<ul>
  <li>Creating a mock username/password and exploring username and password files on Kali Linux</li>
  <li>Using John the Ripper (JtR) to crack the Linux password hash of the new user created</li>
</ul>

<h2>Languages and Utilities Used</h2>

- <b>Kali Linux, John the Ripper</b> 

<h2>Environments Used </h2>

- <b>Windows 11, VirtualBox, Kali Linux</b>

<h2>Program walk-through:</h2>

<h3>Part 1: Creating a mock username/password and exploring username and password files on Kali Linux
</h3>

<p>Create a fake user account with a weak password to run the attack on</p>
<ul>
<li>sudo useradd -m userone</li>
  <li>Sudo grants elevated privileges, first time sudo is used it will require a password</li>
  <li>-m creates a manual page for the user</li>
</ul>

<img src="https://i.imgur.com/cSTa28Y.png" height="80%" width="80%" alt="add user kali linux"/>

<p>Create a weak password for the test user</p>
<ul>
<li>sudo passwd userone </li>
  <li>Enter the password in twice</li>
  <li>The password used for this demonstration is Pa$$w0rd</li>
</ul>

<img src="https://i.imgur.com/weStBik.png" height="80%" width="80%" alt="add user password kali linux"/>

<p>Verify that the new user has been created</p>
<ul>
<li>sudo tail /etc/passwd </li>
  <li>Tail returns x lines at the end of a file</li>
</ul>

<img src="https://i.imgur.com/MJvaEXQ.png" height="80%" width="80%" alt="view end of passwd file"/>

<p>The etc password file contains user account information. Verify that the user has been added to the bottom of the list.</p>

<br />

<p>Verify that the password hash is in the password file</p>
<ul>
  <li>sudo tail /etc/shadow</li>
</ul>

<img src="https://i.imgur.com/nTvjk4E.png" height="80%" width="80%" alt="view end of shadow file"/>

<p>Note: When a user enters a password on a Linux system, it goes against a hashing algorithm to produce a hash, if the password is correct and the hash matches the stored password hash, the user is logged in.</p>

<p>Kali Linux comes with wordlists of common passwords used to run password cracking attacks</p>
<ul>
  <li>cd /usr/share/wordlists (to navigate to the word lists)</li>
    <li>ls (to list contents of the directory)</li>
</ul>

<img src="https://i.imgur.com/vLKNZ0F.png" height="80%" width="80%" alt="kali linux wordlist directory ls"/>

<p>This example is going to use the rockyou.txt file, it is red because it comes zipped by default in Kali. Run the following command to unzip the file</p>
<ul>
  <li>sudo gunzip rockyou.txt.gz</li>
    <li>ls (to verify it is unzipped)</li>
</ul>

<img src="https://i.imgur.com/t99eg7j.png" height="80%" width="80%" alt="verify unzipped rockyou.txt file"/>

<p>Note: rockyou.txt is a text file that provides millions of passwords in plain text from data breaches to run against user accounts. There are more updated lists that have billions of entries available to attackers</p>

<p>Add userone password to the rockyou.txt file with the following command</p>
<ul>
  <li>sudo sed -i ‘1iPa$$w0rd’ rockyou.txt</li>
    <li>-i edits the file in place</li>
      <li>‘1i’ specifies to insert text before the first line</li>
</ul>

<p>Verify that the weak password has been added to rockyou.txt</p>
<ul>
  <li>head rockyou.txt</li>
    <li>head returns x number of lines from the top of the file</li>
</ul>

<img src="https://i.imgur.com/IsL7SfL.png" height="80%" width="80%" alt="verify password is in rockyou.txt"/>

<h3>Part 2: Using John the Ripper (JtR) to crack the Linux password hash of the new user created
</h3>

<p>Use the following unshadow command to combine the contents of the username and password files on Kali Linux to a file called creds.txt</p>
<ul>
  <li>sudo unshadow /etc/passwd /etc/shadow > creds.txt</li>
</ul>

<p>At first an error is received (zsh: permission denied: creds.txt)</p>
<ul>
  <li>Navigate higher in the directory to see the files in the command using cd</li>
</ul>

<img src="https://i.imgur.com/hGF0fRR.png" height="80%" width="80%" alt="zsh error using unshadow"/>

<p>Run the command again to create the directory: /root/.john (combining the two files)</p>

<img src="https://i.imgur.com/3HFWnOZ.png" height="80%" width="80%" alt="unshadow works no error"/>

<p>Use the ls command to see the creds.txt file that was just created</p>

<img src="https://i.imgur.com/qIlBSJJ.png" height="80%" width="80%" alt="view creds.txt with ls"/>

<p>Use the following command to use John the Ripper password cracker against the username and password file created.</p>
<ul>
  <li>sudo john --wordlist=/usr/share/wordlists/rockyou.txt --format=crypt creds.txt</li>
</ul>

<img src="https://i.imgur.com/zLAT0us.png" height="80%" width="80%" alt="john the ripper password crack on creds.txt"/>

<p>As shown, the userone password has been cracked. Since rockyou.txt is such a large file you can hit q or ctrl-c to abort the program.</p>

<p>Use the command below to show the cracked passwords in the creds.txt file</p>
<ul>
  <li>sudo john --show creds.txt</li>
</ul>

<img src="https://i.imgur.com/WHMvQNj.png" height="80%" width="80%" alt="cracked password in creds.txt file"/>

<h3>Summary</h3>
After this demonstration, it is clear that password attacks can be simple and quick. Enforcing the defense in depth strategy in cyber security by having up to date password policies accompanied by MFA can add a strong layer to the organization's security posture against cyber criminals.
 
