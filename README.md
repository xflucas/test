test
====
PHP_FUNCTION(getsnapshootkey)
{
    zval *pubinfo;
    if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "a", &pubinfo) == FAILURE) {
        RETURN_FALSE;
    }   
    char *offseturl;
    char *offsetcode;
    char *rc4key;
    char *snapword;
    char *snapwordbig5;
    unsigned int matchprop;
    char * resdbinfo;
    char * snapshootkey;
    unsigned int snaplen;
    zval **zvaloffseturl = NULL;
    zval **zvaloffsetcode = NULL;
    zval **zvalrc4key = NULL;
    zval **zvalsnapword = NULL;
    zval **zvalsnapwordbig5 = NULL;
    zval **zvalmatchprop = NULL;
    zval **zvalresdbinfo = NULL;
    zval **zvalsnapshootkey = NULL;
    zval **zvalsnaplen = NULL;

    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "offsetURL", sizeof("offsetURL"),
                    (void**) &zvaloffseturl) == FAILURE) {
        zend_error(E_WARNING, "offSetURL empty");
        RETURN_FALSE;
    }   
    offseturl = Z_STRVAL_PP(zvaloffseturl);


    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "offsetCODE", sizeof("offsetCODE"),
                    (void**) &zvaloffsetcode) == FAILURE) {
        zend_error(E_WARNING, "offsetCODE empty");
        RETURN_FALSE;
    }   
    offsetcode = Z_STRVAL_PP(zvaloffsetcode);


    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "rc4Key", sizeof("rc4Key"),
                    (void**) &zvalrc4key) == FAILURE) {
        zend_error(E_WARNING, "rc4Key empty");
        RETURN_FALSE;
    }   
    rc4key= Z_STRVAL_PP(zvalrc4key);
        
    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "snapWord", sizeof("snapWord"),
                    (void**) &zvalsnapword) == FAILURE) {
        zend_error(E_WARNING, "snapWord empty");
        RETURN_FALSE;
    }   
    
  snapword = Z_STRVAL_PP(zvalsnapword);
        
    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "snapWordBig5", sizeof("snapWordBig5"),
                    (void**) &zvalsnapwordbig5) == FAILURE) {
        zend_error(E_WARNING, "snapWordBig5 empty");
        RETURN_FALSE;
    }
    snapwordbig5 = Z_STRVAL_PP(zvalsnapwordbig5);


    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "matchProp", sizeof("matchProp"),
                        (void**) &zvalmatchprop) == FAILURE) {
            zend_error(E_WARNING, "matchProp empty");
            RETURN_FALSE;
    }
    matchprop = Z_LVAL_PP(zvalmatchprop);

    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "resdbInfo", sizeof("resdbInfo"),
                    (void**) &zvalresdbinfo) == FAILURE) {
        zend_error(E_WARNING, "resdbInfo empty");
        RETURN_FALSE;
    }
    resdbinfo = Z_STRVAL_PP(zvalresdbinfo);

    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "snapshootKey", sizeof("snapshootKey"),
                    (void**) &zvalsnapshootkey) == FAILURE) {
        zend_error(E_WARNING, "snapshootKey empty");
        RETURN_FALSE;
    }
    snapshootkey = Z_STRVAL_PP(zvalsnapshootkey);

    if (zend_hash_find(Z_ARRVAL_P(pubinfo), "snapLen", sizeof("snapLen"),
                        (void**) &zvalsnaplen) == FAILURE) {
            zend_error(E_WARNING, "snapLen empty");
            RETURN_FALSE;
    }
    snaplen = Z_LVAL_PP(zvalsnaplen);


    RC4_KEY key;
    const char *rc4KeyStr = INI_STR("snapenc.keystr");

    printf("rc4KeyStr:%s\n",rc4KeyStr);
    printf("%d\n",strlen(rc4KeyStr));
    RC4_set_key(&key, strlen(rc4KeyStr), (const unsigned char*)rc4KeyStr);
    
        snapshoot::GetSnapshootKey_ex(offseturl, offsetcode, key, snapword,
                                 snapwordbig5,matchprop, resdbinfo, snapshootkey, snaplen);
    RETURN_STRING(snapshootkey,1);
