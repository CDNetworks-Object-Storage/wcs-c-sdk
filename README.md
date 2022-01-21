# wcs-c-sdk
C SDK for wcs


## 1 Brief Introduction

The usage of this SDK is compliant with C89 standard.

Includes:
- Pubilc Part: src/wcs/base.c, src/wcs/conf.c, src/wcs/http.c, src/base/log.c
- Upload small file from client: src/wcs/base_io.c io.c
- Multipart upload/breakpoint upload from client: src/base/threadpoo.c, src/base/inifile.c, src/base/patchfileops.c, src/wcs/ multipart_io.c
- Media processing: src/wcs/fop.c

## 2 Install
```
The C-SDK uses cURL, OpenSSL, and it requires the dependency libraries to be installed.

GCC compile options: -lcurl –lssl -lcrypto -lm

If an error such as a connection occurs during project compilation, you need to first check if the dependency libraries and compilation options are correct.
```

Note: Windows environments can be installed using the libraries provided in the [Windows directory](https://github.com/CDNetworks-Object-Storage/wcs-c-sdk/tree/main/windows).


## 3 Related Information

The calling interface uses ACCESS_KEY, SECRET_KEY, upload domain and management domain, which can be obtained from SI portal.

## 4 Log
The problems can be analyzed with the Log, which is divided into six levels. You can choose whether to write to the file or not (for the purpose of performance, it is not recommended to configure writing to the file).
```
> Configuration (LogConfig.ini): [SDKLogConfig]
LOG_LEVEL=5 //0 means print most lines, 5 is recommended
WRITE_FILE=0 //0 means not write to the file	
LOG_FILE=./SDKTest.log //write the file name of log 
```
Notes: If there is no LogConfig.ini, the default setting is LOG_LEVEL=5 and NOT write to the file.
```
> void wcs_Log_Init(char *logConfigFile, FILE *file); // logConfigFile is the path and name of the configuration file
void wcs_close_Logfile(FILE *file);
```
## 5 Interface Calling
```
> Initialize and de-initialize：
wcs_Global_Init (0);
wcs_MacAuth_Init ();
wcs_Client_InitNoAuth (&client, 8192);
……
wcs_Client_Cleanup (&client);	
wcs_Global_Cleanup ();
```

The interface is called in the ellipsis section. This initial call should be made only one time in the main thread, and it is best not to make multiple calls in multiple threads, otherwise unexpected errors can occur. 

## 6 API Description
(Note: The ret para input by each interface should be initialized)

<table>
<thead>
<tr>
<th>Interface</th>
<th>Function Name</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr>
<td>Generate Upload Credential</td>
<td>char *wcs_RS_PutPolicy_Token (wcs_RS_PutPolicy * auth, wcs_Mac * mac)</td>
<td></td>
</tr>
<tr>
<td>Normal Upload</td>
<td>wcs_Error wcs_Io_PutFile (wcs_Client * self, wcs_Io_PutRet * ret, const char *uptoken, const char *key, const char *localFile, wcs_Io_PutExtra * extra)</td>
<td></td>
</tr>
<tr>
<td>Multipart Upload</td>
<td>wcs_Error wcs_Multipart_PutFile (wcs_Client * self, wcs_Multipart_PutRet * ret, const char *uptoken	, const char *key, const char *localFile, wcs_Multipart_PutExtra * extra)</td>
<td>This interface do multiupload through multi-thread, and the max thread can be modified: wcs_Multipart_MaxThreadNum</td>
</tr>
<tr>
<td>Breakpoint Upload</td>
<td>wcs_Error wcs_Multipart_UploadCheck(const char *configFile, wcs_Client * self, 	wcs_Multipart_PutRet * ret)</td>
<td></td>
</tr>
<tr>
<td>Delete File</td>
<td>wcs_Error wcs_RS_Delete (wcs_Client * self, const char *tableName, const char *key, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>Get File Information</td>
<td>wcs_Error wcs_RS_Stat (wcs_Client * self, wcs_RS_StatRet * ret, const char *tableName, const char *key, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>List Resource</td>
<td>wcs_Error wcs_RS_List (wcs_Client * self, wcs_RS_ListRet * ret, const char *bucketName, wcs_Common_Param * param, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>Move Resource</td>
<td>wcs_Error wcs_RS_Move (wcs_Client * self, const char *tableNameSrc, const char *keySrc, const char *tableNameDest, const char *keyDest, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>Update Mirroring Resource</td>
<td>wcs_Error wcs_RS_UpdateMirror(wcs_Client * self, wcs_RS_StatRet * ret, const char *bucketName, const char **fileNameList, unsigned int fileNum, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>Audio/Video Processing</td>
<td>wcs_Error wcs_Fops_Media (wcs_Client * self, wcs_FOPS_Response * ret, wcs_FOPS_MediaParam *param, const char *mgrHost)</td>
</tr>
<tr>
<td>Get Resource</td>
<td>wcs_Error wcs_Fops_Fetch(wcs_Client * self, wcs_FOPS_Response * ret,  wcs_FOPS_FetchParam *ops[], unsigned int opsNum, const char *mgrHost )</td>
</tr>
<tr>
<td>Copy Resource</td>
<td>wcs_Error wcs_RS_Copy (wcs_Client * self, const char *tableNameSrc, const char *keySrc, const char *tableNameDest, const char *keyDest, const char *mgrHost)</td>
<td></td>
</tr>
<tr>
<td>Base64 Coding</td>
<td>wcs_Error wcs_Encode_Base64(int argc, char **argv)</td>
<td></td>
</tr>
</tbody>
</table>

## 7 Example
- [demo](https://github.com/CDNetworks-Object-Storage/wcs-c-sdk/blob/main/demo/test.c)

- fops for video
```
YXZ0aHVtYi9mbHYvdmIvNjRrfHNhdmVzL2FHRnNaWGwwWlhOME9uUmxjM1JXYVdSbGJ3PT07YXZ0aHVtYi9tNGEvdmIvMTI4S3xzYXZlcy9hR0ZzWlhsMFpYTjBPblJsYzNSQmRXUnBidz09
```

- fops for fetchURL
```
fetchURL/aHR0cDovL2ltYWdlcy53Lndjc2FwaS5iaXoubWF0b2Nsb3VkLmNvbS8xLnBuZw==/bucket/aGFsZXl0ZXN0/key/dGVzdE1lZGlh/prefix/bWVkaWE=/md5/aa36f4e18a13762d77cce9e749976b3c/decompression/zip 
multiple fops supported, separated by (;)
```

