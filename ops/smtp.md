# Tsim koj tus kheej SMTP mail xa server

## lus qhuab qhia

SMTP tuaj yeem ncaj qha yuav cov kev pabcuam los ntawm cov neeg muag khoom huab, xws li:

* [Amazon SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali huab email thawb](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Koj tuaj yeem tsim koj tus kheej mail server - xa tsis txwv, tus nqi qis tag nrho.

Hauv qab no, peb qhia cov kauj ruam los ntawm kauj ruam yuav ua li cas los tsim peb tus kheej mail server.

## Server xaiv

Tus kheej-hosted SMTP server xav tau pej xeem IP nrog cov chaw nres nkoj 25, 456, thiab 587 qhib.

Feem ntau siv huab huab tau thaiv cov chaw nres nkoj no los ntawm lub neej ntawd, thiab nws tuaj yeem qhib tau los ntawm kev tshaj tawm txoj haujlwm, tab sis nws muaj teeb meem heev tom qab tag nrho.

Kuv pom zoo kom yuav los ntawm tus tswv tsev uas muaj cov chaw nres nkoj no qhib thiab txhawb kev teeb tsa cov npe rov qab.

Ntawm no, kuv xav kom [Contabo](https://contabo.com) .

Contabo yog ib tug hosting muab kev pab raws li nyob rau hauv Munich, lub teb chaws Yelemees, nrhiav tau nyob rau hauv 2003 nrog cov nqi sib tw heev.

Yog tias koj xaiv Euro raws li cov txiaj ntsig ntawm kev yuav khoom, tus nqi yuav pheej yig dua (ib lub server nrog 8GB nco thiab 4 CPUs raug nqi txog 529 yuan ib xyoos, thiab tus nqi pib pib yog dawb rau ib xyoos).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Thaum tso ib qho kev txiav txim, hais `prefer AMD` , thiab cov neeg rau zaub mov nrog AMD CPU yuav muaj kev ua tau zoo dua.

Hauv qab no, kuv yuav coj Contabo's VPS ua piv txwv los ua qauv qhia yuav ua li cas los tsim koj tus kheej mail server.

## Ubuntu system configuration

Lub operating system ntawm no yog Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Yog tias tus neeg rau zaub mov ntawm ssh qhia `Welcome to TinyCore 13!` (raws li qhia hauv daim duab hauv qab no), nws txhais tau hais tias lub kaw lus tseem tsis tau teeb tsa. Thov rho tawm ssh thiab tos ob peb feeb kom nkag mus dua.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

Thaum `Welcome to Ubuntu 22.04.1 LTS` tshwm, qhov pib ua tiav, thiab koj tuaj yeem txuas ntxiv nrog cov kauj ruam hauv qab no.

### [Optional] Pib qhov kev txhim kho ib puag ncig

Cov kauj ruam no yog xaiv tau.

Txhawm rau kom yooj yim, kuv tso qhov kev teeb tsa thiab kev teeb tsa ntawm ubuntu software hauv [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Khiav cov lus txib nram qab no rau nruab nrog ib nias.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Suav cov neeg siv, thov siv cov lus txib hauv qab no hloov, thiab cov lus, thaj tsam, thiab lwm yam yuav raug txiav.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo enables IPV6

Qhib IPV6 kom SMTP tuaj yeem xa email nrog IPV6 chaw nyob.

kho `/etc/sysctl.conf`

Hloov kho lossis ntxiv cov kab hauv qab no

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Ua raws li [cov lus qhia contabo: Ntxiv IPv6 txuas rau koj lub server](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Kho kom raug `/etc/netplan/01-netcfg.yaml` , ntxiv ob peb kab raws li qhia hauv daim duab hauv qab no (Contabo VPS default configuration file twb muaj cov kab no, tsuas yog uncomment lawv).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Tom qab ntawd `netplan apply` los ua qhov hloov kho kev teeb tsa siv tau.

Tom qab kev teeb tsa tiav, koj tuaj yeem siv `curl 6.ipw.cn` los saib ipv6 chaw nyob ntawm koj lub network sab nraud.

## Clone lub configuration repository ops

```
git clone https://github.com/wactax/ops.soft.git
```

## Tsim ib daim ntawv pov thawj SSL dawb rau koj lub npe sau npe

Xa ntawv yuav tsum muaj daim ntawv pov thawj SSL rau kev encryption thiab kos npe.

Peb siv [acme.sh](https://github.com/acmesh-official/acme.sh) los tsim daim ntawv pov thawj.

acme.sh yog qhov qhib qhov chaw siv daim ntawv pov thawj kos npe,

Nkag mus rau lub tsev khaws khoom configuration ops.soft, khiav `./ssl.sh` , thiab `conf` nplaub tshev yuav raug tsim nyob rau hauv **sab sauv directory** .

Nrhiav koj tus kws kho mob DNS los ntawm [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , kho `conf/conf.sh` .

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Tom qab ntawd khiav `./ssl.sh 123.com` los tsim `123.com` thiab `*.123.com` daim ntawv pov thawj rau koj lub npe sau npe.

Thawj qhov kev khiav yuav cia li nruab [acme.sh](https://github.com/acmesh-official/acme.sh) thiab ntxiv cov haujlwm tau teem tseg rau kev rov ua haujlwm tsis siv neeg. Koj tuaj yeem pom `crontab -l` , muaj cov kab hauv qab no.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Txoj kev rau daim ntawv pov thawj generated yog ib yam zoo li `/mnt/www/.acme.sh/123.com_ecc。`

Daim ntawv pov thawj txuas ntxiv yuav hu rau `conf/reload/123.com.sh` tsab ntawv, kho cov ntawv no, koj tuaj yeem ntxiv cov lus txib xws li `nginx -s reload` kom rov ua dua daim ntawv pov thawj cache ntawm cov ntawv thov cuam tshuam.

## Tsim SMTP server nrog chasquid

[chasquid](https://github.com/albertito/chasquid) yog qhov qhib SMTP server sau ua lus Go.

Raws li kev hloov pauv rau cov kev pabcuam xa ntawv thaum ub xws li Postfix thiab Sendmail, chasquid yooj yim dua thiab siv tau yooj yim dua, thiab nws kuj yooj yim dua rau kev txhim kho theem nrab.

Khiav `./chasquid/init.sh 123.com` yuav raug ntsia tau nrog ib nias (hloov 123.com nrog koj lub npe xa).

## Configure Email Kos npe DKIM

DKIM yog siv los xa email kos npe los tiv thaiv cov ntawv los ntawm kev ua spam.

Tom qab cov lus txib ua tiav, koj yuav raug ceeb toom kom teeb tsa DKIM cov ntaub ntawv (raws li qhia hauv qab no).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Tsuas yog ntxiv TXT cov ntaub ntawv rau koj DNS (raws li qhia hauv qab no).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Saib cov xwm txheej pabcuam & cov ntawv teev npe

 `systemctl status chasquid` Saib kev pabcuam xwm txheej.

Lub xeev ntawm kev ua haujlwm ib txwm muaj raws li qhia hauv daim duab hauv qab no

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` lossis `journalctl -xeu chasquid` tuaj yeem pom qhov yuam kev.

## Reverse domain name configuration

Lub npe thim rov qab yog tso cai rau tus IP chaw nyob kom daws tau lub npe sau npe.

Teem lub npe rov qab tuaj yeem tiv thaiv email los ntawm kev txheeb xyuas tias spam.

Thaum tau txais kev xa ntawv, tus neeg rau zaub mov tau txais yuav ua qhov rov qab sau npe tshuaj ntsuam xyuas ntawm tus IP chaw nyob ntawm tus xa server kom paub meej tias tus xa server puas muaj lub npe rov qab siv tau.

Yog hais tias tus xa neeg rau zaub mov tsis muaj lub npe thim rov qab lossis yog lub npe rov qab sau npe tsis sib xws IP chaw nyob ntawm tus xa server, tus neeg rau zaub mov tau txais yuav lees paub email yog spam lossis tsis lees paub.

Mus saib [https://my.contabo.com/rdns](https://my.contabo.com/rdns) thiab teeb tsa raws li qhia hauv qab no

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Tom qab teeb tsa lub npe thim rov qab, nco ntsoov teeb tsa kev daws teeb meem rau pem hauv ntej ntawm lub npe sau npe ipv4 thiab ipv6 rau lub server.

## Kho lub hostname ntawm chasquid.conf

Hloov kho `conf/chasquid/chasquid.conf` rau tus nqi ntawm lub npe rov qab.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Tom qab ntawd khiav `systemctl restart chasquid` kom rov pib qhov kev pabcuam.

## Backup conf rau git repository

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Piv txwv li, kuv thaub qab conf folder rau kuv tus kheej github txheej txheem raws li hauv qab no

Tsim ib lub tsev khaws khoom ntiag tug ua ntej

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Nkag mus rau conf directory thiab xa mus rau lub tsev khaws khoom

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Ntxiv tus xa

khiav

```
chasquid-util user-add i@wac.tax
```

Muaj peev xwm ntxiv sender

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Xyuas kom tseeb tias tus password raug teeb tsa kom raug

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Tom qab ntxiv tus neeg siv, `chasquid/domains/wac.tax/users` yuav hloov kho, nco ntsoov xa mus rau lub tsev khaws khoom.

## DNS ntxiv SPF cov ntaub ntawv

SPF (Sender Policy Framework) yog ib qho kev txheeb xyuas email siv los tiv thaiv email kev dag.

Nws txheeb xyuas tus kheej ntawm tus neeg xa ntawv los ntawm kev txheeb xyuas tias tus xa tus IP chaw nyob sib tw DNS cov ntaub ntawv ntawm lub npe sau npe uas nws tau lees tias yog, tiv thaiv cov neeg dag ntxias los ntawm kev xa email cuav.

Ntxiv SPF cov ntaub ntawv tuaj yeem tiv thaiv email los ntawm kev txheeb xyuas spam ntau li ntau tau.

Yog tias koj lub npe server tsis txhawb hom SPF, tsuas yog ntxiv TXT hom ntaub ntawv.

Piv txwv li, SPF ntawm `wac.tax` yog raws li nram no

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF rau `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Nco ntsoov tias kuv muaj `include:_spf.google.com` ntawm no, qhov no yog vim kuv yuav teeb tsa `i@wac.tax` raws li qhov chaw xa ntawv hauv Google mailbox tom qab.

## DNS configuration DMARC

DMARC yog cov ntawv luv ntawm (Domain-based Message Authentication, Reporting & Conformance).

Nws yog siv los ntes SPF bounces (tej zaum tshwm sim los ntawm kev teeb tsa tsis raug, lossis lwm tus neeg ua txuj ua koj xa spam).

Add TXT record `_dmarc` ,

Cov ntsiab lus yog raws li nram no

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Lub ntsiab lus ntawm txhua qhov parameter yog raws li hauv qab no

### p (Txoj cai)

Qhia txog yuav ua li cas thiaj lis cov emails uas tsis ua tiav SPF (Sender Policy Framework) lossis DKIM (DomainKeys Identified Mail) pov thawj. Lub p parameter tuaj yeem raug teeb tsa rau ib qho ntawm peb qhov tseem ceeb:

* tsis muaj: Tsis muaj kev nqis tes ua, tsuas yog cov txiaj ntsig kev pov thawj tau rov qab rau tus neeg xa ntawv los ntawm email qhia txog txheej txheem.
* Quarantine: Muab cov ntawv xa ntawv uas tsis tau dhau qhov kev pov thawj rau hauv spam folder, tab sis yuav tsis lees txais cov ntawv ncaj qha.
* tsis lees paub: ncaj qha tsis lees txais email uas tsis lees paub.

### fo (Kev xaiv ua tsis tiav)

Qhia txog tus nqi ntawm cov ntaub ntawv xa rov qab los ntawm kev tshaj tawm cov txheej txheem. Nws tuaj yeem raug teeb tsa rau ib qho ntawm cov txiaj ntsig hauv qab no:

* 0: Tshaj tawm cov txiaj ntsig kev lees paub rau txhua cov lus
* 1: Tsuas yog tshaj tawm cov lus uas tsis ua pov thawj
* d: Tsuas yog tshaj tawm lub npe pov thawj tsis ua tiav
* s: tsuas yog qhia txog qhov tsis ua pov thawj SPF
* l: Tsuas yog tshaj tawm DKIM kev pov thawj tsis ua tiav

### rau & ruf

* rua (Tshaj Qhia URI rau Kev Tshaj Tawm Tshaj Tawm): Email chaw nyob kom tau txais cov ntaub ntawv sib sau ua ke
* ruf (Tshaj Qhia URI rau Forensic reports): email chaw nyob kom tau txais cov lus qhia ntxaws

## Ntxiv MX cov ntaub ntawv xa mus rau email rau Google Mail

Vim kuv nrhiav tsis tau ib lub mailbox pub dawb uas txhawb nqa qhov chaw nyob thoob ntiaj teb (Catch-All, tuaj yeem tau txais ib qho emails xa mus rau lub npe sau npe no, tsis muaj kev txwv ntawm cov ntawv sau ua ntej), kuv siv chasquid xa tag nrho email rau kuv lub thawv xa ntawv Gmail.

**Yog tias koj muaj koj tus kheej lub thawv xa ntawv them nyiaj, thov tsis txhob hloov MX thiab hla cov kauj ruam no.**

Kho kom raug `conf/chasquid/domains/wac.tax/aliases` , teeb tsa xa ntawv xa mus

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` qhia tag nrho cov emails, `i` yog email chaw nyob ua ntej ntawm tus neeg xa khoom tsim los saum toj no. Txhawm rau xa xa ntawv, txhua tus neeg siv yuav tsum tau ntxiv ib kab.

Tom qab ntawd ntxiv cov ntaub ntawv MX (Kuv taw tes ncaj qha mus rau qhov chaw nyob ntawm lub npe rov qab ntawm no, raws li pom hauv thawj kab hauv daim duab hauv qab no).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Tom qab kev teeb tsa tiav lawm, koj tuaj yeem siv lwm qhov chaw nyob email xa email rau `i@wac.tax` thiab `any123@wac.tax` saib seb koj puas tuaj yeem tau txais email hauv Gmail.

Yog tias tsis yog, kos lub log chasquid ( `grep chasquid /var/log/syslog` ).

## Xa email rau i@wac.tax nrog Google Mail

Tom qab Google Mail tau txais cov ntawv, kuv ib txwm cia siab tias yuav teb nrog `i@wac.tax` es tsis yog i.wac.tax@gmail.com.

Mus saib [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) thiab nyem "Add another email address".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Tom qab ntawd, sau tus lej pov thawj tau txais los ntawm email uas tau xa mus rau.

Thaum kawg, nws tuaj yeem raug teeb tsa raws li qhov chaw nyob xa mus (nrog rau kev xaiv teb nrog tib qhov chaw nyob).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Ua li no, peb tau ua tiav kev tsim SMTP mail server thiab tib lub sijhawm siv Google Mail xa thiab tau txais email.

## Xa email kuaj xyuas seb qhov kev teeb tsa puas ua tiav

Nkag mus rau `ops/chasquid`

Khiav `direnv allow` rau nruab dependencies (direnv tau raug teeb tsa hauv cov txheej txheem pib ib qho tseem ceeb dhau los thiab tus nuv tau ntxiv rau lub plhaub)

ces khiav

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Lub ntsiab lus ntawm cov parameter yog raws li nram no

* neeg siv: SMTP username
* pass: SMTP password
* rau: tus txais

Koj tuaj yeem xa email kuaj.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Nws raug nquahu kom siv Gmail kom tau txais email xeem los xyuas seb cov kev teeb tsa puas ua tiav.

### TLS tus qauv encryption

Raws li pom hauv daim duab hauv qab no, muaj qhov ntsuas me me no, uas txhais tau tias daim ntawv pov thawj SSL tau ua tiav tiav.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Ces nyem "Show Original Email"

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Raws li pom nyob rau hauv daim duab hauv qab no, Gmail thawj nplooj ntawv xa tuaj qhia DKIM, uas txhais tau hais tias DKIM teeb tsa ua tiav.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Txheeb xyuas qhov Txais hauv header ntawm thawj email, thiab koj tuaj yeem pom tias tus neeg xa ntawv chaw nyob yog IPV6, uas txhais tau hais tias IPV6 kuj tau teeb tsa tiav.
