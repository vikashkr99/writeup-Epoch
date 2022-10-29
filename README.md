<h1>writeup-Epoch</h1>
<img src="./img/logo.png" alt="logo" width="700">
<p>
    The room description suggests tha there is a vulnerable WebApplication, So let's start the machine and check out the WebApplication.<br>
    The room also suggests Command Injection so maybe that's the vulnerability in the WebApplication.
</p>

<h3>WebApplication</h3>
<p>
    So this is what the web application looks like.<br>
    <img src="./img/webPage.png" alt="webPage" width="500"><br>
    Let's try command injection from <a href="https://book.hacktricks.xyz/pentesting-web/command-injection">hacktricks</a><br>
    <img src="./img/webPage2.png" alt="webPage2" width="500"><br>
    It works.<br>
    Let's get a reverse shell. <br>
    <img src="./img/shell.png" alt="shell" width="500"><br><br>
    It looks like we are in a docker container but trying to get out of it isn't working. Apart from that, there are a few things i tried that didn't work which are : <br>
    <ol>
        <li><code>docker ps</code></li>
        <li><code>find / -type f -perm -04000 2>/dev/null</code></li>
        <li><code>sudo -l</code></li>
        <li><code>Nothing in crontab</code></li>
    </ol>
    Nothing intresting in <code>find</code> and the rest gave <code>not found</code> error. <br>
    So i decided to look at the enviorment variables which i later realised was also in the hint and there was our flag.<br>
</p>
