# **Upload Vulnerabilities**

---

### **Introduction**

The ability to upload files to a server has become an integral part of how we interact with web applications. Be it a profile picture for a social media website, a report being uploaded to cloud storage, or saving a project on GitHub; the applications for file upload features are limitless.

Unfortunately, when handled badly, file uploads can also open up severe vulnerabilities in the server. This can lead to anything from relatively minor, nuisance problems; all the way up to full **Remote Code Execution (RCE)** if an attacker manages to upload and execute a shell. With unrestricted upload access to a server (and the ability to retrieve data at will), an attacker could deface or otherwise alter existing content -- up to and including injecting malicious webpages, which lead to further vulnerabilities such as **XSS or CSRF**. By uploading arbitrary files, an attacker could potentially also use the server to host and/or serve illegal content, or to leak sensitive information. Realistically speaking, an attacker with the ability to upload a file of their choice to your server -- with no restrictions -- is very dangerous indeed.

- **IMP Definition**
    
    ### XSS
    
    - A type of security vulnerability typically found in web applications. It allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can then steal sensitive information, like user's cookies, session tokens, or other sensitive data.
    
    ### CSRF
    
    - Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

### Overwriting Existing Files

When files are uploaded to the server, a range of checks should be carried out to ensure that the file will not overwrite anything which already exists on the server. Common practice is to assign the file with a new name -- often either random, or with the date and time of upload added to the start or end of the original filename. Alternatively, checks may be applied to see if the filename already exists on the server; if a file with the same name already exists then the server will return an error message asking the user to pick a different file name. File permissions also come into play when protecting existing files from being overwritten. Web pages, for example, should not be writeable to the web user, thus preventing them from being overwritten with a malicious version uploaded by an attacker.
If, however, no such precautions are taken, then we might potentially be able to overwrite existing files on the server. Realistically speaking, the chances are that file permissions on the server will prevent this from being a serious vulnerability. That said, it could still be quite the nuisance, and is worth keeping an eye out for in a ***pentest or bug hunting environment***.

### **Remote Code Execution**

It's all well and good overwriting files that exist on the server. That's a nuisance to the person maintaining the site, and may lead to some vulnerabilities, but let's go further; let's go for RCE!

Remote Code Execution (as the name suggests) would allow us to execute code  arbitrarily on the web server. Whilst this is likely to be as a  low-privileged web user account (such as `www-data` on Linux servers), it's still an extremely serious vulnerability. Remote code  execution via an upload vulnerability in a web application tends to be exploited by uploading a program written in the same language as the  back-end of the website (or another language which the server understands and will execute). Traditionally this would be PHP, however, in more recent times, other back-end languages have become more common  **(Python Django and JavaScript in the form of Node.js being prime examples)**. It's worth noting that in a *routed* application (i.e. an application where the routes are defined programmatically rather than being mapped to the file-system), this method of attack becomes a lot more complicated and a lot less likely to occur. Most modern web frameworks are routed programmatically.

There are two basic ways to achieve RCE on a webserver when exploiting a file upload 
vulnerability: webshells, and reverse/bind shells. Realistically a fully featured reverse/bind shell is the ideal goal for an attacker; however, a webshell may be the only option available (for example, if a file 
length limit has been imposed on uploads, or if firewall rules prevent any network-based shells). We'll take a look at each of these in turn. As a general methodology, we would be looking to upload a shell of one 
kind or another, then activating it, either by navigating directly to the file if the server allows it (non-routed applications with inadequate restrictions), or by otherwise forcing the webapp to run the script for us (necessary in routed applications).

---

*Web shells:*

Let's assume that we've found a webpage with an upload form:

![upload from](https://i.imgur.com/GxMJAKH.png)

Where do we go from here? Well, let's start with a **gobuster** scan:

![upload image](https://i.imgur.com/OftwAIE.png)

Looks like we've got two directories here -- `uploads` and `assets`.Of these, it seems likely that any files we upload will be placed in the "uploads" directory. We'll try uploading a legitimate image file first. Here I am choosing our  cute dog photo from the previous task:

![upload image](https://i.imgur.com/aAyIrod.png)

![upload image](https://i.imgur.com/mIbGRIk.png)

Now, if we go to `http://demo.uploadvulns.thm/uploads` we should see that the spaniel picture has been uploaded!

![up](https://i.imgur.com/lVe2tjL.png)

![up](https://i.imgur.com/N8vWlVO.png)

Ok, we can upload images. Let's try a webshell now.

As it is, we know that this webserver is running with a PHP back-end, so we'll skip straight to creating and uploading the shell. In real life, we may need to do a little more enumeration; however, PHP is a good place to start regardless.A simple webshell works by taking a parameter and executing it as a system command. In PHP, the syntax for this would be:

```php
**<?php
echo system($_GET["cmd"]);
?>**
```

This code takes a GET parameter and executes it as a system command. It then echoes the output out to the screen.

Let's try uploading it to the site, then using it to show our current user and the contents of the current directory:

![up](https://i.imgur.com/CU0Uyx5.png)

Success!

We could now use this shell to read files from the system, or upgrade from here to a reverse shell. Now that we have RCE, the options are limitless. Note that when using webshells, it's usually easier to view the output by looking at the source code of the page. This drastically improves the formatting of the output.

---

*Reverse Shells:*

The process for uploading a reverse shell is almost identical to that of uploading a webshell, so this section will be shorter. We'll be using the ubiquitous Pentest Monkey reverse shell, which comes by default on Kali Linux, but can also be downloaded [here](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php). You will need to edit line 49 of the shell. It will currently say `$ip = '127.0.0.1';  // CHANGE THIS` -- as it instructs, change `127.0.0.1` to your TryHackMe tun0 IP address, which can be found on the [access page](https://tryhackme.com/access).
You can ignore the following line, which also asks to be changed. With the shell edited, the next thing we need to do is start a Netcat listener to receive the connection. `nc -lvnp 1234`:

![up](https://i.imgur.com/ysY306E.png)

Now, let's upload the shell, then activate it by navigating to `http://demo.uploadvulns.thm/uploads/shell.php`. The name of the shell will obviously be whatever you called it (`php-reverse-shell.php` by default).

The website should hang and not load properly -- however, if we switch back to our terminal, we have a hit!

![up](https://i.imgur.com/he0hbiR.png)

Once again, we have obtained RCE on this webserver. From here we would want to stabilize our shell and escalate our privileges, but those are tasks for another time. For now, it's time you tried this for yourself!

---

### Filtering

Up until now we have largely been ignoring the counter-defences employed by web developers to defend against file upload vulnerabilities. Every website that you've successfully attacked so far in this room has been completely insecure. It's time that changed. From here on out, we'll be looking at some of the defence mechanisms used to prevent malicious file uploads, and how to circumvent them.

First up, let's discuss the differences between ***client-side filtering and server-side filtering.***

When we talk about a script being **"Client-Side"**, in the context of web applications, we mean that it's running in the user's browser as opposed to on the web server itself. *JavaScript* is pretty much ubiquitous as the client-side scripting language, although alternatives do exist.  Regardless of the language being used, a client-side script will be run in your web browser. In the context of file-uploads, this means that the filtering occurs before the file is even uploaded to the server. Theoretically, this would seem like a good thing, right? In an ideal world, it would be; however, because the filtering is happening on our computer, it is trivially easy to bypass. As such client-side filtering by itself is a highly insecure method of verifying that an uploaded file is not malicious.

Conversely, as you may have guessed, a server-side script will be run on the server. Traditionally PHP was the predominant server-side language (with Microsoft's ASP for IIS coming in close second); however, in recent years, other options (C#, Node.js, Python, Ruby on Rails, and a variety of others) have become more widely used. Server-side filtering tends to be more difficult to bypass, as you don't have the code in front of you. As the code is executed on the server, in most cases it will also be impossible to bypass the filter completely; instead we have to form a payload which conforms to the filters in place, but still allows us to execute our code.

With that in mind, let's take a look at some different kinds of filtering.

***Extension Validation:***

- File extensions are used (in theory) to identify the contents of a file. In practice they are very easy to change, so actually don't mean much; however, MS Windows still uses them to identify file types, although Unix based systems tend to rely on other methods, which we'll cover in a bit. Filters that check for extensions work in one of two ways. They either blacklist extensions (i.e. have a list of extensions which are not allowed) or they whitelist extensions (i.e. have a list of extensions which are allowed, and reject everything else).

***File Type Filtering:***

Similar to Extension validation, but more intensive, file type filtering looks, once again, to verify that the contents of a file are acceptable to upload. We'll be looking at two types of file type validation:

1. ***MIME validation:*** *MIME (Multipurpose Internet Mail Extension)* types are used as an identifier for files -- originally when transfered as attachments over email, but now also when files are being transferred over HTTP(S). The MIME type for a file upload is attached in the header of the request, and looks something like this:

![uptWRKW](https://github.com/user-attachments/assets/dc0fc93c-e709-437b-abe8-e219800690af)

  MIME types follow the format <type>/<subtype>. In the request above, you can see that the image      "spaniel.jpg" was uploaded to the server. As a legitimate JPEG image, the MIME type for this upload was **"image/jpeg"**. The MIME type for a file can be checked client-side and/or server-side; however, as MIME is based on the extension of the file, this is extremely easy to bypass.

1. ***Magic Number validation:*** Magic numbers are the more accurate way of determining the contents of a file; although, they are by no means impossible to fake. The "magic number" of a file is a string of bytes at the very beginning of the file content which identify the content. For example, a PNG file would have these bytes at the very top of the file: 89 50 4E 47 0D 0A 1A 0A.

Unlike Windows, Unix systems use magic numbers for identifying files; however, when dealing with file uploads, it is possible to check the magic number of the uploaded file to ensure that it is safe to accept. This is by no means a guaranteed solution, but it's more effective than checking the extension of a file.

![vHQWOgi](https://github.com/user-attachments/assets/2f0c1045-4b16-44da-a469-b17782bb386b)


***File Length Filtering:***

- File length filters are used to prevent huge files from being uploaded to the server via an upload form (as this can potentially starve the server of resources). In most cases this will not cause us any issues when we upload shells; however, it's worth bearing in mind that if an upload form only expects a very small file to be uploaded, there may be a length filter in place to ensure that the file length requirement is adhered to. As an example, our fully fledged PHP reverse shell from the previous task is **5.4Kb big** -- relatively tiny, but if the form expects a maximum of **2Kb** then we would need to find an alternative shell to upload.

***File Name Filtering:***

- As touched upon previously, files uploaded to a server should be unique. Usually this would mean adding a random aspect to the file name, however, an alternative strategy would be to check if a file with the same name already exists on the server, and give the user an error if so. Additionally, file names should be sanitized on upload to ensure that they don't contain any "bad characters", which could potentially cause problems on the file system when uploaded (e.g. null bytes or forward slashes on Linux, as well as control characters such as ; and potentially unicode characters). What this means for us is that, on a well administered system, our uploaded files are unlikely to have the same name we gave them before uploading, so be aware that you may have to go hunting for your shell in the event that you manage to bypass the content filtering.

***File Content Filtering:***

- More complicated filtering systems may scan the full contents of an uploaded file to ensure that it's not spoofing its extension, MIME type and Magic Number. This is a significantly more complex process than the majority of basic filtration systems employ, and thus will not be covered in this room.

> It's worth noting that none of these filters are perfect by themselves -- they will usually be used in conjunction with each other, providing a **multi-layered filter, thus** increasing the security of the upload significantly. Any of these filters can all be applied client-side, server-side, or both.
> 

Similarly, different frameworks and languages come with their own inherent methods of filtering and validating uploaded files. As a result, it is possible for language specific exploits to appear; for example, until PHP major version five, it was possible to bypass an extension filter by appending a null byte, followed by a valid extension, to the malicious **.php file**. More recently it was also possible to inject PHP code into the **exif data o**f an otherwise valid image file, then force the server to execute it. These are things that you are welcome to research further, should you be interested.

---

### **Bypassing Client-Side Filtering**

We'll begin with the first (and weakest) line of defence: Client-Side Filtering.

As mentioned previously, client-side filtering tends to be extremely easy to bypass, as it occurs entirely on a machine that *you* control. When you have access to the code, it's very easy to alter it.

There are four easy ways to bypass your average client-side file upload filter:

1. *Turn off Javascript in your browser* - this will work provided the site doesn't require Javascript in order to provide basic functionality. If turning off Javascript completely will
prevent the site from working at all then one of the other methods would be more desirable; otherwise, this can be an effective way of
completely bypassing the client-side filter.
2. *Intercept and modify the incoming page.* Using Burpsuite, we can intercept the incoming web page and strip out the Javascript filter before it has a chance to run. The process for this will be covered below.
3. *Intercept and modify the file upload*. Where the previous method works *before* the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter). Again, we will cover the process for using this method in the course of the task.
4. *Send the file directly to the upload point.* Why use the webpage with the filter, when you can send the file directly using a tool like `curl`? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter. We will not be covering this method in any real depth in this tutorial, however, the syntax for such a command would look something like this: **`curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>`**. To use this method you would first aim to intercept a successful upload (using Burpsuite or the browser console) to see the parameters being used in the upload, which can then be slotted into the above command.

We will be covering methods two and three in depth below.

---

Let's assume that, once again, we have found an upload page on a website:

![up](https://i.imgur.com/fI67jX0.png)

As always, we'll take a look at the source code. Here we see a basic Javascript function checking for the MIME type of uploaded files:

![up](https://i.imgur.com/TrI5jQD.png)

In this instance we can see that the filter is using a *whitelist* to exclude any MIME type that isn't `image/jpeg`.

Our next step is to attempt a file upload -- as expected, if we choose a  JPEG, the function accepts it. Anything else and the upload is rejected. Having established this, let's start [Burpsuite](https://blog.tryhackme.com/setting-up-burp/) and  reload the page. We will see our own request to the site, but what we really want to see is the server's *response*, so right click on the intercepted data, scroll down to "Do Intercept", then select "Response to this request":

![up](https://i.imgur.com/T0RjAry.png)

When e click the "Forward" button at the top of the window, we will then see the server's response to our request. Here we can delete, comment out, or otherwise break the Javascript function before it has a chance 
to load:

![up](https://i.imgur.com/ACgWLpH.png)

Having deleted the function, we once again click "Forward" until the site has  finished loading, and are now free to upload any kind of file to the website:

![up](https://i.imgur.com/5cyqjqa.png)

It's worth noting here that Burpsuite will not, by default, intercept any external Javascript files that the web page is loading. If you need to edit a script which is not inside the main page being loaded, you'll need to go to the "Options" tab at the top of the Burpsuite window, then under the "Intercept Client Requests" section, edit the condition of the first line to remove `^js$|`:

![up](https://i.imgur.com/95hi6pX.png)

---

We've already bypassed this filter by intercepting and removing it prior to the page being loaded, but let's try doing it by uploading a file with a legitimate extension and MIME type, then intercepting and correcting the upload with Burpsuite.

Having reloaded the webpage to put the filter back in place, let's take the reverse shell that we used before and rename it to be called "shell.jpg". As the MIME type (based on the file extension) automatically checks out, the Client-Side filter lets our payload through without complaining:

![up](https://i.imgur.com/WNpruFM.png)

Once again we'll activate our Burpsuite intercept, then click "Upload" and catch the request:

![op](https://i.imgur.com/h2164Li.png)

Observe that the MIME type of our PHP shell is currently `image/jpeg`. We'll change this to `text/x-php`, and the file extension from `.jpg` to `.php`, then forward the request to the server:

![iop](https://i.imgur.com/sqmwssT.png)

Now, when we navigate to **`http://demo.uploadvulns.thm/uploads/shell.php` hav**ing set up a **netcat** listener, we receive a connection from the shell!

![uiop](https://i.imgur.com/cUqNO2L.png)

---

### Bypassing Server-Side Filtering: File Extensions

Time to turn things up another notch!

Client-side filters are easy to bypass -- you can see the code for them, even if it's been obfuscated and needs prcessed before you can read it; but what happens when you *can't* see or manipulate the code? Well, that's a server-side filter. In short, we have to perform a lot of testing to build up an idea of what is or is not allowed through the filter, then gradually put together a payload which conforms to the restrictions.

For the first part of this task we'll take a look at a website that's using a blacklist for file extensions as a server side filter. There are a variety of different ways that this could be coded, and the bypass we use is dependent on that. In the real world we wouldn't be able to see the code for this, but for this example, it will be included here:

```php
<?php
    //Get the extension
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];
    //Check the extension against the blacklist -- .php and .phtml
    switch($extension){
        case "php":
        case "phtml":
        case NULL:
            $uploadFail = True;
            break;
        default:
            $uploadFail = False;
    }
?>
```

In this instance, the code is looking for the last period (`.`) in the file name and uses that to confirm the extension, so that is what we'll be trying to bypass here. Other ways the code could be working include: searching for the first period in the file name, or splitting the file name at each period and checking to see if any blacklisted extensions show up. We'll cover this latter case later on, but in the meantime, let's focus on the code we've got here.

We can see that the code is filtering out the `.php` and `.phtml` extensions, so if we want to upload a PHP script we're going to have to find another extension. **The [wikipedia page](https://en.wikipedia.org/wiki/PHP) for PHP gives us a few common extensions that we can try; however, there are actually a variety of other more rarely used extensions available that webservers may nonetheless still recognise. These include: `.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht` and `.phar`. Many of these bypass the filter (which only blocks`.php` and `.phtml`), but it appears that the server is configured not to recognize them as PHP files, as in the below example:**

![tyuiop](https://i.imgur.com/yzOGVob.png)

This is actually the default for Apache2 servers, at the time of writing; however, the sysadmin may have changed the default configuration (or the server may be out of date), so it's well worth trying.

Eventually we find that the `.phar` extension bypasses the filter -- and works -- thus giving us our shell:

![op](https://i.imgur.com/Aigaz4R.png)

---

Let's have a look at another example, with a different filter. This time we'll do it completely black-box: i.e. without the source code.Once again, we have our upload form:

![iop](https://i.imgur.com/STsI51E.png)

Ok, we'll start by scoping this out with a completely legitimate upload. Let's try uploading the `spaniel.jpg` image from before:

![uiop](https://i.imgur.com/tp6T2WH.png)

Well, that tells us that JPEGS are accepted at least. Let's go for one that we can be pretty sure will be rejected (`shell.php`):

![vbn](https://i.imgur.com/hk4inJ2.png)

Can't say that was unexpected.

From here we enumerate further, trying the techniques from above and just generally trying to get an idea of what the filter will accept or reject.

In this case we find that there are no shell extensions that both execute, and are not filtered, so it's back to the drawing board.

In the previous example we saw that the code was using the `pathinfo()` PHP function to get the last few characters after the `.`, but what happens if it filters the input slightly differently?

Let's try uploading a file called `shell.jpg.php`. We already know that JPEG files are accepted, so what if the filter is just checking to see if the `.jpg` file extension is somewhere within the input?

Pseudocode for this kind of filter may look something like this:

`ACCEPT FILE FROM THE USER -- SAVE FILENAME IN VARIABLE userInput`

`IF STRING ".jpg" IS IN VARIABLE userInput:`

    `SAVE THE FILE`

`ELSE:`

    `RETURN ERROR MESSAGE`

When we try to upload our file we get a success message. Navigating to the `/uploads` directory confirms that the payload was successfully uploaded:

![vbn](https://i.imgur.com/K55eu9o.png)

Activating it, we receive our shell:

![axx](https://i.imgur.com/VVAKZfw.png)

---

This is by no means an exhaustive list of upload vulnerabilities related to file extensions. As with everything in hacking, we are looking to exploit flaws in code that others have written; this code may very well be uniquely written for the task at hand. This is the really important point to take away from this task: there are a million different ways to implement the same feature when it comes to programming -- your 
exploitation must be tailored to the filter at hand. The key to bypassing any kind of server side filter is to enumerate and see what is allowed, as well as what is blocked; then try to craft a payload which can pass the criteria the filter is looking for.

---

### Bypassing Server-Side Filtering: Magic Numbers

We've already had a look at server-side extension filtering, but let's also take the opportunity to see how magic number checking could be implemented as a server-side filter.As mentioned previously, 
magic numbers are used as a more accurate identifier of files. The magic number of a file is a string of hex digits, and is always the very first thing in a file. Knowing this, it's possible to use magic numbers 
to validate file uploads, simply by reading those first few bytes and comparing them against either a whitelist or a blacklist. Bear in mind that this technique can be very effective against a PHP based webserver; however, it can sometimes fail against other types of webserver (hint hint).

Let's take a look at an example. As per usual, we have an upload page:

![up](https://i.imgur.com/yQnQGsn.png)

As expected, if we upload our standard shell.php file, we get an error; however, if we upload a JPEG, the website is fine with it. All running as per expected so far.

From the previous attempt at an upload, we know that JPEG files are accepted, so let's try adding the JPEG magic number to the top of our `shell.php` file. A quick look at the [list of file signatures on Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures) shows us that there are several possible magic numbers of JPEG files. 
It shouldn't matter which we use here, so let's just pick one **(`FF D8 FF DB`)**. We could add the ASCII representation of these digits **(ÿØÿÛ**) directly to the top of the file but it's often easier to work directly with the hexadecimal representation, so let's cover that method.

Before we get started, let's use the Linux `file` command to check the file type of our shell:

![mmm](https://i.imgur.com/2126EHS.png)

As expected, the command tells us that the filetype is PHP. Keep this in mind as we proceed with the explanation.

We can see that the magic number we've chosen is four bytes long, so let's open up the reverse shell script and add four random characters on the first line. These characters do not matter, so for this example we'll 
just use four "A"s:

![up](https://i.imgur.com/oe434wu.png)

Save the file and exit. Next we're going to reopen the file in `hexeditor`(which comes by default on Kali), or any other tool which allows you to see and edit the shell as hex. In hexeditor the file looks like this:

![and](https://i.imgur.com/otIyN96.png)

Note the four bytes in the red box: they are all `41`, which is the hex code for a capital "A" -- exactly what we added at the top of the file previously.

Change this to the magic number we found earlier for JPEG files: `FF D8 FF DB`

![op](https://i.imgur.com/2OlGKdQ.png)

Now if we save and exit the file (Ctrl + x), we can use

once again, and see that we have successfully spoofed the filetype of our shell:

![DD](https://i.imgur.com/ldyt88v.png)

Perfect. Now let's try uploading the modified shell and see if it bypasses the filter!

![sss](https://i.imgur.com/Coat5LI.png)

There we have it -- we bypassed the server-side magic number filter and received a reverse shell.

---
