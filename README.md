# ipfs-cluster-client

> A client library for the IPFS Cluster HTTP API, implemented in JavaScript. 

> The code is mostly from `js-ipfs-api` modified to consume the `ipfs-cluster` API.


**UNOFFICIAL AND ALPHA**

## Install

This module can be installed through npm directly from the github repository.

`npm install ipfs-cluster-client`

## Usage

To import the module:

```
var ipfsClusterClient = require('ipfs-cluster-client')
```

```
// connect to ipfs-cluster daemon API server (displayed are default values)
var ipfsCluster = ipfsClusterClient('localhost', 9094, {protocol: 'http'})
```

### API

The API is currently a work-in-progress. The exposed methods are designed
to be similar to `ipfs-cluster-ctl` provided in `ipfs/ipfs-cluster`.

### Examples

Use Javascript:
```js
const axios = require('axios');
const FormData = require('form-data');
const fs = require('fs');
const path = require('path');

const API_URL = 'http://ipfs.peopledata.org.cn:9094'; // 根据您的 IPFS Cluster 配置调整

// 测试添加文件
const filePath = path.join(__dirname, 'test_ipfs.txt'); 
const formData = new FormData();
formData.append('file', fs.createReadStream(filePath));

axios.post(`${API_URL}/add`, formData, {
  headers: formData.getHeaders()
})
  .then(response => {
    console.log('File added:', response.data);
    const cid = response.data.cid;

    // 这里可以添加对 /pins/{cid} 端点的调用以 pin 文件
    console.log(cid);

  })
  .catch(error => {
    console.error('Error adding file:', error.message);
  });

```

Use `requests``:
```js
const http = require('https'); 
const hostname = 'ipfs.peopledata.org.cn';

const options = {
  hostname: hostname,
  path: '/peers', 
  method: 'GET' 
};

const req = http.request(options, res => {
  console.log(`状态码: ${res.statusCode}`);

  res.on('data', d => {
    process.stdout.write(d);
  });
});


req.on('error', error => {
  console.error(error);
});


req.end();


```


Use Python:
```python

import requests

# IPFS Cluster的API地址
CLUSTER_API_URL = "https://ipfs.peopledata.org.cn"

# 添加文件到IPFS Cluster
def add_file_to_cluster(filepath):
    with open(filepath, 'rb') as f:
        files = {'file': f}
        response = requests.post(f"{CLUSTER_API_URL}/add", files=files)
        return response.json()

# 获取Cluster的ID信息
def get_cluster_id():
    response = requests.get(f"{CLUSTER_API_URL}/id")
    return response.json()

# 固定CID到Cluster
def pin_cid_to_cluster(cid):
    response = requests.post(f"{CLUSTER_API_URL}/pins/{cid}")
    return response.json()

cluster_info = get_cluster_id()
cluster_info

  
# 测试添加文件到Cluster
result = add_file_to_cluster("说明.md")
print(result)

# 如果你已经有一个CID并想固定到Cluster，你可以这样做：
cid = result['cid']
pin_result = pin_cid_to_cluster(cid)
print(pin_result)

```


## License

MIT
