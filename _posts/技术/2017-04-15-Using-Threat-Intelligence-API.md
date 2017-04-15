---
layout: post
title: 几个威胁情报库的API调用方法
category: 技术
tags: 威胁情报
keywords:
description:
---
>本文基于实际情况，对IBM xForce、AlienVault和Virustotal的公共API调用情况，进行分析和测试，目前已经解决以上三家威胁情报厂家的API调用问题

### 1、IBM xForce

##### 1.1 IBM xForce支持的调用资源

对于利用IBM xForce威胁情报进行分析，IBM支持的调用资源的类型比较多，包括通过domain、IP对域名解析记录PassiveDNS进行查询，通过hash（包括MD5、Sha1、Sha256）对恶意样本信息进行查询，通过domain对whois信息进行查询，通过URL对URL的情况进行查询，通过IP对IP地理位置进行的查询等。

##### 1.2 官方的API调用方法

在官方介绍中，通过post的方法，使用curl -u {apikey:password} API_URL的方法，进行官方API的调用，如：

```
curl -u {apikey:password} https://api.xforce.ibmcloud.com/url/cnn.com
```

而在实际使用curl调用API，并对返回结果进行处理和分析时，发现对返回的数据难以进行标准的格式化处理，需要处理较多的闲杂数据。

而对于curl -u {apikey:password}的调用方式，则可以使用requests.get(url,auth=(apikey, apipassword),timeout = 5)的方式进行替代。

##### 1.3 便于Python开发的调用方法及相关代码

IBM Xforce的调用代码如下：

```
elif company == "ibm":
    apikey = "fe998eab-8325-45cb-bac1-.........."
    apipassword = "51aaccf2-8b95-46e1-9f68-..........."

    if type == 'domain':
        url = "https://api.xforce.ibmcloud.com/resolve/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    elif type == 'ip':
        url = "https://api.xforce.ibmcloud.com/resolve/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    elif type == 'hash' or 'md5' or 'sha256' or 'sha1':
        url = "https://api.xforce.ibmcloud.com/malware/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    # return {"error": "Invalid input."}
    elif type == 'whois':
        url = "https://api.xforce.ibmcloud.com/whois/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    elif type == "url":
        url = "https://api.xforce.ibmcloud.com/url/malware/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    elif type == "geo":
        url = "https://api.xforce.ibmcloud.com/ipr/" + value
        response = requests.get(url,auth=(apikey, apipassword),timeout = 5)
        json_response = response.json()
        return HttpResponse(json.dumps(json_response))

    else:
        return HttpResponse(company+type+value)
```

### 2、AlienVault

##### 2.1 AlienVault支持的调用资源

AlienVault支持的资源有：通过Domain和IP地址查询PassiveDNS解析记录，通过IP地址查询Geo地理位置，通过Domain查询Whois信息，通过Hash（包括Md5、Sha1、Sha256）查询恶意代码相关信息，通过URL查询URL相关信息等。

##### 2.2 官方的API调用方法

AlienValut官方通过没有明确给出，介绍的只是利用Get的方式，通过/api/v1/indicators/IPv4/{ip}/{section}对相关资源进行调用。

另外还介绍了之前的调用方式，如下：

```
curl https://otx.alienvault.com:443/api/v1/pulses/subscribed?page=1 -H "X-OTX-API-KEY: f95af845c6a3e86cf6855a25719b538d9d2570ba207d1caa668................"
```

通过curl的方式对资源调用，同样会遇到对返回数据的复杂处理问题，该问题比较难以解决。

##### 2.3 便于Python开发的调用方法及相关代码

通过对AlienValut的OTX-Python-SDK的代码进行分析，发现了其是通过在header中嵌入key的方式，进行调用。

相关资源调用代码如下：

```
elif company == 'alienvault':
    apikey = "f95af845c6a3e86cf6855a25719b538d9d2570ba207d1caa6683454e0f2abfbc"
    if type == 'domain':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/domain/" + value + "/passive_dns"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)

    elif type == 'ip':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/IPv4/" + value + "/passive_dns"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception, e:
            return HttpResponse(e)

    elif type == 'geo':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/IPv4/" + value + "/geo"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)

    elif type == 'whois':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/domain/" + value + "/whois"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)

    elif type == 'hash' or 'md5' or 'sha256' or 'sha1':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/file/" + value + "/general"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)

    elif type == 'url':
        headers = {'X-OTX-API-KEY': apikey, 'User-Agent': 'OTX Python {}/1.1', 'Content-Type': 'application/json'}
        url = "https://otx.alienvault.com/api/v1/indicators/url/" + value + "/general"
        try:
            response = requests.get(url,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)
    else:
        return HttpResponse(company+type+value)
```

### 3、VirusTotal

##### 3.1 VirusTotal支持的调用资源

VirusTotal支持的调用类型有：通过hash（包括Md5、Sha1、Sha256）对恶意代码相关的信息进行查找和分析，通过URL实现对URL的检测及分析结果导出，通过IP和Domain查询Passive DNS域名解析记录信息。

##### 3.2 官方的API调用方法

VirusTotal官方的API实现文档中，给出了较为详细和全面的调用方法，支持Python、cURL和PHP调用。

示例代码如下：

```
import requests
params = {'apikey': '-YOUR API KEY HERE-'}
files = {'file': ('myfile.exe', open('myfile.exe', 'rb'))}
response = requests.post('https://www.virustotal.com/vtapi/v2/file/scan', files=files, params=params)
jon_response = response.json()
```

##### 3.3 便于Python开发的调用方法及相关代码

实际对API调用的方法，也基本查考官方的API调用方法，代码如下：

```
elif company == 'virustotal':
    apikey = "ac0683da633ce102bbfda1fc6bff7414f24c07e8d1629208544e18516df762f0"

    if type == 'domain':
        params = {}
        url = 'https://www.virustotal.com/vtapi/v2/domain/report'
        params['domain'] = value
        params['apikey'] = apikey
        response = urllib.urlopen('%s?%s' % (url, urllib.urlencode(params))).read()
        response_dict = json.loads(response)
        return HttpResponse(json.dumps(response_dict))

    elif type == 'ip':
        params = {}
        url = 'https://www.virustotal.com/vtapi/v2/ip-address/report'
        params['ip'] = value
        params['apikey'] = apikey
        response = urllib.urlopen('%s?%s' % (url, urllib.urlencode(params))).read()
        response_dict = json.loads(response)
        return HttpResponse(json.dumps(response_dict))

    elif type == 'url':
        params = {}
        params['apikey'] = apikey
        params['resource'] = value
        headers = {"Accept-Encoding": "gzip, deflate","User-Agent" : "gzip,  My Python requests library example client or username"}
        try:
            response = requests.post('https://www.virustotal.com/vtapi/v2/url/report',data=params,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception, e:
            return HttpResponse(e)

    elif type == 'md5' or 'sha1' or 'sha256' or 'hash':
        params = {}
        params['apikey'] = apikey
        params['resource'] = value
        headers = {"Accept-Encoding": "gzip, deflate","User-Agent" : "gzip,  My Python requests library example client or username"}
        try:
            response = requests.get('https://www.virustotal.com/vtapi/v2/file/report',params=params,headers=headers)
            json_response = response.json()
            return HttpResponse(json.dumps(json_response))
        except Exception,e:
            return HttpResponse(e)
```

### 参考

【1】IBM X-Force Exchange API Doc，https://api.xforce.ibmcloud.com/doc/

【2】AlienVault API Documentation，https://otx.alienvault.com/api/

【3】OTX-Python-SDK，https://github.com/AlienVault-Labs/OTX-Python-SDK

【3】VirusTotal Public API v2.0，https://www.virustotal.com/en/documentation/public-api/ 

