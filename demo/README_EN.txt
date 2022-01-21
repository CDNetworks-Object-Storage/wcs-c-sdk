Test description (The following parameters do not indicate the type is all strings. For string parameters, if not set, you need to pass NULL, and the test only pays attention to the parameters between /* */. The function name is used by the product to write documents):
Parameter settings: Specify the parameters required by the test interface to specify the interface
The first parameter of the test is the interface to be tested, and the corresponding relationship is as follows:
1 Generate upload token
2 Upload small-sized files
3 Multipart upload (file size must be larger than 4MB,)
4 Resume from a breakpoint
5 Delete File
6 Query File information
7 List Resources
8 Move Resources
9 Update Mirror Resource
10 Video Process
11 Fetch Resources
12 Copy Resources
13 Base64 encoding

wcs_Init_Log()


Interface parameter description:

1. Generate Token
char *wcs_RS_PutPolicy_Token (wcs_RS_PutPolicy * auth, wcs_Mac * mac)；
/*
*	@param 
		IF:
		SK:
		const char *scope;
		const char *saveKey;
		const char *returnUrl;
		const char *returnBody;
		const char *callbackUrl;
		const char *callbackBody; (need to escape $ when using magic variable, please refer to the following example)
		const char *persistentOps;
		const char *contentDetect;
		const char *persistentNotifyUrl;
		const char *detectNotifyURL
		const char *detectNotifyRule;
		wcs_Bool overwrite;
		wcs_Bool separate;
		wcs_Uint64 fsizeLimit;
		wcs_Uint32 expires;
	例如： ./wcsTest 1 yourAk yourSk yourBucketname NULL NULL "TestParam=\$(x:MyTest)&Parma1=\$(x:MyParam)&Param2=\$(x:MyParam2)" NULL  NULL NULL NULL NULL NULL  NULL 1 1	
*/

2. Upload file
wcs_Error wcs_Io_PutFile (wcs_Client * self, cJSON ** ret, const char *uptoken, const char *key, const char *localFile, wcs_Io_PutExtra * extra)；
/*
*	@param uptoken key localFile upHost numXParam key value
	For example: (numXParam = 3 followed by 3 sets of key values)
	2 youruploadtoken fop.c /home/haley/fop.c http://test.up0.v1.wcsapi.com/file/upload 3 x:MyTest MyTestValue x:MyParam ParamValue x:MyParam2 Param2Value
*/

3. Multipart upload (blockSize, chunkSize are both Byte)
wcs_Error wcs_Multipart_PutFile (wcs_Client * self, wcs_Multipart_PutRet * ret, const char *uptoken
	, const char *key, const char *localFile, wcs_Multipart_PutExtra * extra)；
 /*
 *	 @param uptoken key localFile upHost blockSize chunkSize
 */
For example: ./wcsTest 3 youruploadtoken text 00.tar.gz /root/api_test/test_tool/csdk/c-sdk-refactor-build.tar.gz http://test.up0.v1.wcsapi.com NULL NULL 1 x :MyTest MyTestValue


4. Resume from a breakpoint
wcs_Error wcs_Multipart_UploadCheck(const char *configFile, wcs_Client * self, 
	wcs_Multipart_PutRet * ret)
/*
	@param 
*/

5. Delete File
wcs_Error wcs_RS_Delete (wcs_Client * self, const char *tableName, const char *key, const char *mgrHost)
/*
* @param AK SK bucketName keyName mgrHost
*/

6. Get File Status
wcs_Error wcs_RS_Stat (wcs_Client * self, cJSON ** ret, const char *tableName, const char *key, const char *mgrHost)
/*
*	@param accessKey secretKey bucketName keyName mgrHost
*/

7. List Resource
wcs_Error wcs_RS_List (wcs_Client * self, cJSON ** ret, const char *bucketName, wcs_Common_Param * param, const char *mgrHost)
/*
	@param accessKey secretKey bucketName mgrHost prefix limit mode marker
*/

8. Move Resource
wcs_Error wcs_RS_Move (wcs_Client * self, const char *tableNameSrc, const char *keySrc,
	const char *tableNameDest, const char *keyDest, const char *mgrHost)；
/*
@param accessKey secretKey srcBucketName srcKey destBucketName destKey mgrHost
*/

9. Update Mirror Resource
wcs_Error wcs_RS_UpdateMirror(wcs_Client * self, cJSON ** ret, const char *bucketName, 
const char **fileNameList, unsigned int fileNum, const char *mgrHost)；
/*
*	@param accessKey secretKey bucketName mgrHost fileNum fileName1 fileName2 ....fileNameN
	the N is equal fileNum
*/

10. Video processing
wcs_Error wcs_Fops_Media (wcs_Client * self, wcs_FOPS_Response * ret, 
	wcs_FOPS_MediaParam *param, const char *apiHost)
/*
*	@param
	AK SK mgrHost
	char *bucket;
	char * key;	
	//The splicing and base64 encoding of fops are done by the customer themselves, and then input through a string
	char *fops;

	char *notifyURL;
	int force;
	wcs_Bool separate;
*/

11. Fetch Resources
wcs_Error wcs_Fops_Fetch(wcs_Client * self, wcs_FOPS_Response * ret, 
	wcs_FOPS_FetchParam *ops,const char *apiHost)
/*
*	@parm
	AK SK apiHost
	char *ops; //The splicing and encoding are done by the customer themselves

	//Public section
	char *notifyURL;
	int force;
	wcs_Bool separate;
*/

12. Copy Resources
wcs_Error wcs_RS_Copy (wcs_Client * self, const char *tableNameSrc, const char *keySrc, 
	const char *tableNameDest, const char *keyDest, const char *mgrHost)
/*
* @param accessKey secretKey srcBucketName srcKey destBucketName destKey mgrHost
*/

13. base64 encoding
char *wcs_String_Encode (const char *buf)
/*
	string
*/


For the meaning of the return value of each interface, please refer to: https://curl.se/libcurl/c/libcurl-errors.html