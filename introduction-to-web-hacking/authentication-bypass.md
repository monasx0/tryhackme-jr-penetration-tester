# Authentication Bypass

## Username Enumeration
Website error messages can be a great source for collecting information to build a list of valid usernames. For example, if we put `admin` in the username field and try to log in website responses with an error called "username already registered". We can use this to our advantage by running a tool called `ffuf` that can check multiple usernames that are already registered on the website at once from the provided wordlist.
#### Example usage of `ffuf`:
```
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.82.184.149/customers/signup -mr "username already exists"
```

Here, `-w` argument selects the file's location on the computer that contains the list of usernames that we're going to check exists. The `-X` argument specifies the request method, this will be a GET request by default, but it is a POST request in our example. The `-d` argument specifies the data that we are going to send. In our example, we have the fields username, email, password and cpassword. We've set the value of the username to **FUZZ**. In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request. The `-H` argument is used for adding additional headers to the request. In this instance, we're setting the `Content-Type` so the web server knows we are sending form data. The `-u` argument specifies the URL we are making the request to, and finally, the `-mr` argument is the text on the page we are looking for to validate we've found a valid username.
## Brute Force 
A brute force attack is an automated process that tries a list of commonly used passwords against either a single username or a list of usernames.
### Example usage of trying brute force attack using ffuf:
```
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.82.184.149/customers/login -fc 200
```
In this command, we've chosen `W1` for our list of valid usernames and `W2` for the list of passwords we will try. The multiple wordlists are again specified with the `-w` argument but separated with a comma.  For a positive match, we're using the `-fc` argument to check for an HTTP status code other than 200. Using `-fc 200` filters out normal-looking responses so that unusual status codes reveal real, interesting endpoints during brute forcing.
## Logic Flaws
A logic flaw is when a typical logical path of an application is either bypassed or manipulated. 
### **Logic Flaw Example**
The application only checks if the user visits the confirmation page.
```
if ($order_confirmed == true) {     echo "Order placed successfully"; }
```
An attacker can directly visit `/order/confirm`. Order will be marked as successful without paying.
## Cookie Tampering
Examining and editing the cookies set by the web server during your online session can have multiple outcomes, such as unauthenticated access, access to another user's account, or elevated privileges.
For example, if these were the cookie set after a successful login:
```
Set-Cookie: logged_in=true; Max-Age=3600; Path=/
Set-Cookie: admin=false; Max-Age=3600; Path=/
```
We see one cookie (logged_in), which appears to control whether the user is currently logged in or not, and another (admin), which controls whether the visitor has admin privileges.
First we will try to send a request to the target page:
```
curl http://ip_address/cookie-test
```
**Result**: Not logged In
Now we'll send another request with the "logged_in cookie" set to true and the admin cookie set to false:
```
curl -H "Cookie: logged_in=true; admin=false" http://ip_address/cookie-test
```
**Output**: Logged In As A User
Lastly, we will send a request to the page setting both "logged_in"  and "admin=true" .
```
curl -H "Cookie: logged_in=true; admin=true" http://ip_address/cookie-test
```
**Output**: Logged In As An Admin
### Hashing
Sometimes cookie values can look like a long string of random characters; these are called hashes which are an irreversible representation of the original text.
**Example**:

| Original string | Hash Method |                              Output                              |
| :-------------: | :---------: | :--------------------------------------------------------------: |
|        1        |     md5     |                 c4ca4238a0b923820dcc509a6f75849b                 |
|        1        |   sha-256   | 6b86b273ff34fce19d6b804eff5a3f5747ada4eaa22f1d49c01e52ddb7875b4b |
|        1        |    sha1     |             356a192b7913b04c54574d18c28d46e6395428ab             |

You can see from the above table that the hash output from the same input string can significantly differ depending on the hash method in use. Even though the hash is irreversible, the same output is produced every time, which is helpful for us as services such as [crack station](https://crackstation.net/) keep databases of billions of hashes and their original strings.
### Encoding
Encoding allows us to convert binary data into human-readable text that can be easily and safely transmitted over mediums that only support plain text ASCII characters.
Common encoding types are `base32` which converts binary data to the characters A-Z and 2-7, and `base64` which converts using the characters a-z, A-Z, 0-9,+, / and the equals sign for padding.
Below is an example which is set by the web server upon logging in:
```
****Set-Cookie:** session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/**
```
This string `base64` decoded has the value of `{"id":1,"admin": false}` we can then encode this back to `base64` encoded again but instead setting the admin value to true, which now gives us admin access.****
