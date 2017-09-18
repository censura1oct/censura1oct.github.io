---
layout: post
title:  "Methods"
date:   2017-09-16 00:00:00 +0200
categories: en
---
The following paragraphs try to document the methods to censor online content used by the Spanish police and the main telecom operators, and alternative ways to access some domain names.

Censorship has targeted official websites of Generalitat, the Catalan Governement. Non-official replicas published by private individuals are still active.
Websites hosted in Spain or domain names managed by organizations in Spain have been disabled physically.

The Spanish police seems not to be able to censor websites hosted abroad or with domain names managed by organizations abroad.

This is the most accurate information available as of September 17, 2017.

## referendum.cat garanties.cat

This was the official website of the Referendum made for the Catalan Governement.
It doesn't work since Guardia Civil (Spanish half civilian half militarized police) seized the hosting provided by [CDMON (hosting)](https://blog.cdmon.com/comunicado-oficial-referendum-cat/) and [Fundació puntCAT (domain name manager)](http://fundacio.cat/ca/noticies/la-fundacio-puntcat-te-com-missio-basica-la-divulgacio-i-presencia-de-la-llengua-i-cultura).

## referendum.*

Non-official, made by private individuals that cloned the contents of referendum.cat.
They have not been seized.

Domain names list:

[https://github.com/GrenderG/referendum_cat_mirror](https://github.com/GrenderG/referendum_cat_mirror)

## ref1oct.org

Non-official, working.

## ref1oct.eu

Since September 15, 2017 it is not available to users of major Spanish telecom operators:

### Orange and telecoms that use their network (Jazztel, SomConnexió, etc.)

~~Working (It is not confirmed that they will take any action against websites).~~

Update September 18, 2017: They have applied censorship that we are still studying.

### Parlem

Working. ([They have announced](https://twitter.com/parlem_telecom/status/909160184517464064) that no content will be censored).

### Euskaltel

They are doing *DNS spoofing*. Some solutions:

- Change the client DNS to 8.8.8.8.
- Use a VPN tunnel

### Adamo

Working.

### Vodafone

They are doing *DNS spoofing*. The client's panel has some personal settings that can be changed by they don't seem to affect the problem. Some solutions

- Without the router provided by Vodafone: change the DNS to 8.8.8.8
- With the router provided by Vodafone: change DNS, ask them to disable the DNS proxy.
- Use a VPN tunnel

### Movistar

The most complex implementation. They are not doing *DNS spoofing*. Instead, they are inspecting user's traffic looking for a pair of IP address and server name of ref1oct.eu (both elements -- one isn't enough to apply the filter).

Actions Movistar is taking:

1. For plain HTTP connections: they check hostname and IPs
2. For HTTPS (SSL/TLS), they look up the field SNI (unencrypted hostname in the TLS message) and the IPs.
3. Screenshot of a TLS package with Wireshark, where you can see the SNI field
4. They use some sort of regular expression:
  - ref1oct.eu doesn't work
  - www.ref1oct.eu doesn't work
  - *.www.ref1oct.eu doesn't work
  - *.ref1oct.eu works
  - Another valid regex: ((.*\.)?www\.)?ref1oct\.eu
5. They are looking for the IP addresses used by Cloudfare, a content delivery network service.
  - 104.20.44.157
  - 104.20.55.157

There are several solutions to be able to reach this domain:

- Use any IP that belongs to Cloudfare for the domain ref1oct.eu, except the two excluded by Movistar. Experiments show that valid IPs are in the subnet 104.20.0.0/16.
- If the domain ref1oct.eu had a subdomain that wasn't www, it would work. For example info.ref1oct.eu
- Using a VPN tunnel

Content injected by Movistar with a 403 response:

```
<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01//EN\">
<html>
  <head>
    <meta charset="utf-8"/>
    <title>

    </title>

  </head>
  <body>
    <CENTER>
      <h1 id="causa" name="PHISHING_TSOL_MENSAJE_1">
      </h1>
      <script type="text/javascript">
        var name = document.getElementById("causa").getAttribute('name')
            var text = ""
                switch (name) {
                  case "PHISHING_TSOL_MENSAJE_1":
                    text = "Judicial_Guardia_Civil"
                      window.location.replace("http://paginaintervenida.edgesuite.net");
                    break;
                  case "Administrativo_Ley_del_Juego":
                    text = "Administrativo_Ley_del_Juego"
                      window.location.replace("http://195.235.52.40");
                    break;
                  case "Judicial_Guardia_Civil":
                    text = "Judicial_Guardia_Civil"
                      window.location.replace("http://paginaintervenida.edgesuite.net");
                    break;
                  default:
                    text = "ERROR 404 - Files not found";
                }
        document.getElementById("causa").innerHTML = text
      </script>
    </CENTER>
  </body>
</html>
```

This code redirects to [http://paginaintervenida.edgesuite.net](http://paginaintervenida.edgesuite.net) (hostead in Akamai), which transfers 497.7 KB (despite the fact that it only displays an image of 83.96 KB).

## Learn more

- [Twitter conversation about Movistar](https://twitter.com/jmendeth/status/909429033838014464).
- [Twitter conversation about Vodafone](https://twitter.com/mola_io/status/909071359107530752).
