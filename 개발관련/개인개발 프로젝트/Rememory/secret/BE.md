## env
```
PORT=5000  
  
DB_URI=mongodb+srv://admin:BFt8a5CmcMvZ8Knw@cluster0.rohzrzo.mongodb.net/  
DB_NAME=rememory-local  
RSA_PRIVATE_KEY_PATH=./.privateKey  
RSA_PUBLIC_KEY_PATH=./.publicKey

```

## RSA 
### generate
```typescript
const { privateKey, publicKey } = generateKeyPairSync('rsa', {  
    modulusLength: 2048,  
    publicKeyEncoding: {  
        type: "pkcs1",  
        format: "pem",  
    },  
    privateKeyEncoding: {  
        type: "pkcs1",  
        format: "pem",  
    },  
})
```
### key
```
-----BEGIN RSA PRIVATE KEY-----  
MIIEowIBAAKCAQEAld7AdUB333SvV/ZlPIjXk52TlyM/LBmzMC7ilEwcTtGHOT2t  
CaBPwvjzWbcmQbOusuDQpngR942CIYvj6NU0vgGAxJTc47c09byh5PDVhdlP7MVD  
RON6AgQ0AWcy4Y8FRa814PWyNWWy9SpTMK8BH3LKnH81LuUdUFeWpcw0OHU3BojK  
Ba1+whuVoyGXbhqq0qZvAqurnMC/q/9VQjYrC6tXYhhCew7gA+6kLnPhIRdladlV  
XpVd9zB3+9qsvp3suvcqGKv+S67V0Zf//lYk381iD7lTTsgT6amgSb43uj8zbzW4  
j988YRnPjXmPXpoGCVfdi5KYFmN1KAaaracPywIDAQABAoIBABAxEzj3wJu3RxRp  
TfG21RehMCdlivcwRaBAiIE+IdbQI1xnUfEWzVdCv0PjmjICAC9aQ6Cgm0xUEQmf  
y/1FP6ABmJBkhuLhUcr02Mbb0C3YVx69BzIDo5RhMN8v75rX8VlgfyExWlITjTyY  
mIlLdwMXS1WSFsRcw4iBFgkOoVux/+5jujuQsXswMD6jShHUgYfu7ZXch9TVO7NJ  
8aOZ81Uo1CQqq1ksMC2WB38MlrfbWpSt7PAS+UpmdgfcYMFTohhthhmhZ6hPZj7o  
Ti8QiO9l15KrofWEK3Equ6GjMJ8dsEVBPlLATzbURyIpnFOPisvRdO54Cbdr/BXt  
VsaqMZECgYEAxn2uEBuW72zwRbdWrdGV1OzsMtaHCeX0NwXPHLfQlxYgv+/j5T+h  
vnjP4KB8XfOqmANLqtADidLJUeZ476LYKs7BLA/Y4xsf1o074+wL/X6ivXIU+sc7  
caBvgPZi5OjM6lMYqfC8eHms9vEDbphR3qlDalpPPKaJPqjpWDy8gdsCgYEAwUrO  
OuOZ0j40R/2+H8AKAHRqzOk7nqVQvFU2bJXDtiiNF/GD8TRtyYfb0Zkix/KK/pty  
6CXm+ufVSAWjqhCtPy6EGwaVMoU3B0hRJrS2AZAtTTDlflxpOJKnHu699v1mhWIg  
3mPj1m3DAaHEekoqDzXXSJ8yCCwEY15urLhu5NECgYBl77uJyDF+qmrG0v4v7DfP  
jxFKloPpYHBIJbKU5A262gFdsRxP6prtT+wqRyE3uuC8isy8X3HBwT/k0MEBCJeN  
fHsWXtka4R47uHKufdY2jGeVdVYy6Eit9R/ukhp9xtUd7ij3dYvFL2/Vrjb+ADnj  
aPgXUWPqGPjY9jRIPYDuCwKBgQCv+TuyD14OM4WDeTZrT3mLmnFVJo2ZzGWpYGbR  
CrQIFfkGMGHv6cx96oss0h8BLAZw7/L3+PHFweTB0iiDfvVLDT1GIYMZYICNx7/h  
3inJWIp1uStmFBnTYGh31+DoiSCaFJFaBlT59inQRYdL0lNiT6E0w4JYQEKqeOGH  
q82B0QKBgGN/9RLTZ+C5ouGMTpYB2TkT3AJrLVAn3+9uzBdvVVQEkxLyuhxhUx+e  
WzJH/PFGLzaHtnTuXmcpbiXgslIG60KR1viwAg16kxwqWBmI2ip0eNEPtJ7jmLuR  
4I3CZLbyxenNgmXAha1icxmTkjIHUMMh1OBYpMD82434z7tt0SfK  
-----END RSA PRIVATE KEY-----

-----BEGIN RSA PUBLIC KEY-----  
MIIBCgKCAQEAld7AdUB333SvV/ZlPIjXk52TlyM/LBmzMC7ilEwcTtGHOT2tCaBP  
wvjzWbcmQbOusuDQpngR942CIYvj6NU0vgGAxJTc47c09byh5PDVhdlP7MVDRON6  
AgQ0AWcy4Y8FRa814PWyNWWy9SpTMK8BH3LKnH81LuUdUFeWpcw0OHU3BojKBa1+  
whuVoyGXbhqq0qZvAqurnMC/q/9VQjYrC6tXYhhCew7gA+6kLnPhIRdladlVXpVd  
9zB3+9qsvp3suvcqGKv+S67V0Zf//lYk381iD7lTTsgT6amgSb43uj8zbzW4j988  
YRnPjXmPXpoGCVfdi5KYFmN1KAaaracPywIDAQAB  
-----END RSA PUBLIC KEY-----
```