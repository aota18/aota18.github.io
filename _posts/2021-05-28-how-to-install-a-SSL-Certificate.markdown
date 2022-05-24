---
layout: post
title: "How to install an SSL Certificate"
date: 2021-05-28 12:11:00 +0900
categories: [Web, SSL]
tags: ["Web", "SSL"]
---

## Index

- What is a SSL?
- SSL Issuing
- Install SSL

## What is a SSL?

### HTTP and HTTPS

In the past, most sites used to start with `http://~`, but now they are all addressed with URLs starting with `https://~`. This means that the computer or mobile phone we connect to is in encrypted communication with the server on that site. HTTPS is followed by an s, which means 'Secure'.

### SSL (Secure Socket Layer)

HTTPS was born by strengthening the security of HTTP in the communication protocol. ![](https://images.velog.io/images/sapphire317/post/3e1f4a1b-119d-4050-b6a7-83c119f36d6a/Screen%20Shot%202021-05-28%20at%203.39.44%20PM.png)
[Source : https://blog.naver.com/skinfosec2000/222135874222]

SSL creates an independent protocol layer called the 'security layer' and falls between the application layer (HTTP, FTP...) and the transport layer (TCP, etc.) as shown. In other words, HTTPS is a protocol for secure HTTP communication by placing an HTTP protocol on top of SSL. It's like being shipped to a paper delivery box and then turned into a locked iron box.

## SSL Issuing

There are several subjects of SSL issuance, but there is a site that can be received free of charge, so I received it from the site below.

Site : https://letsencrypt.org/ko/docs/

### Issuing Wildcard Certificate

A wildcard certificate is a certificate that supports multiple subdomains on a per-host basis in a Domain. To issue this certificate, additional information must be added to the 'DNS record' either through the domain server or through the hosting organization's domain management tools.

## Install SSL

After SSL issuance, the 'SSL Certificate' and 'Private Key' files are saved. You can configure the nginx server settings with these two files.

Below is the contents of default.conf of nginx.

```
server {
    listen 443 ssl http2 default_server;
    server_name abc.com;

    ### Cert file ###
    ssl_certificate /usr/share/nginx/ssl/abc.com.pem;
    ### Private Key 파일 ###
    ssl_certificate_key /usr/share/nginx/ssl/abc.com.priv.pem;
    ### SSL Activation ###
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1.1 TLSv1.2;


    ### redirects to m.abc.com으로 on Mobile ###
    set $mobile_rewrite do_not_perform;
    if ($http_user_agent ~* "(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge\ |maemo|midp|mmp|mobile.+firefox|netfront|opera\ m(ob|in)i|palm(\ os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows\ ce|xda|xiino [NC,OR]") {
        set $mobile_rewrite perform;
    }
    if ($http_user_agent ~* "^(1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a\ wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r\ |s\ )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1\ u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp(\ i|ip)|hs\-c|ht(c(\-|\ |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac(\ |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt(\ |\/)|klon|kpt\ |kwc\-|kyo(c|k)|le(no|xi)|lg(\ g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-|\ |o|v)|zz)|mt(50|p1|v\ )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v\ )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-|\ )|webc|whit|wi(g\ |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-) [NC]") {
        set $mobile_rewrite perform;
    }

    if ($mobile_rewrite = perform) {
        rewrite ^ https://m.abc.com$request_uri? redirect;
        break;
    }


    ### location of page source ###
    root /usr/share/nginx/html;
    index index.html index.htm;
}

```

When you have finished setting up HTTPS (port 443), you need to set up Redirect to turn the existing HTTP user back to HTTPS. The contents are as follows.

```
server {
    listen 80;
    server_name abc.com;
    ### Redirects to HTTPS ###
    return 301 https://$server_name$request_uri;

}
```

If you write down the Nginx setting and restart it, you can see that it runs properly.
