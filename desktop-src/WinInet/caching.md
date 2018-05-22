---
title: Caching
description: The WinINet functions have simple, yet flexible, built-in caching support.
ms.assetid: '44c268c9-a745-432a-8540-60d7e7d2cb2d'
---

# Caching

The WinINet functions have simple, yet flexible, built-in caching support. Any data retrieved from the network is cached on the hard disk and retrieved for subsequent requests. The application can control the caching on each request. For http requests from the server, most headers received are also cached. When an http request is satisfied from the cache, the cached headers are also returned to the caller. This makes data download seamless, whether the data is coming from the cache or from the network.

Applications must properly allocate a buffer in order to get the desired results when using the persistent URL caching functions. For more information, see [Using Buffers](appendix-b-using-buffers.md).

## Cache Behavior During Response Processing

The WinINet cache is compliant with the HTTP cache-control directives described in RFC 2616. The cache-control directives and application set flags determine what may be cached; however, WinINet determines what is actually cached based on the following criterion:

-   WinINet only caches HTTP and FTP responses.
-   Only well behaved responses may be stored by a cache and used in a reply to a subsequent Request. Well behaved responses are defined as responses that return successfully.
-   By default, WinINet will cache successful responses unless either a cache-control directive from the server, or an application-defined flag specifically denote that the response may not be cached.
-   In general, responses to the GET verb are cached if the requirements listed above are met. Responses to PUT and POST verbs are not cached under any circumstances.
-   Items will be cached even when the cache is full. If an added item is puts the cache over the size limit, the cache scavenger is scheduled. By default, items are not guaranteed to remain more than 10 minutes in the cache. For more information, see the [Cache Scavenger](#cache-scavenger) section below.
-   Https is cached by default. This is managed by a global setting that cannot be overridden by application-defined cache directives. To override the global setting, select the Internet Options applet in the control panel, and go to the advanced tab. Check the "Do not save encrypted pages to disk" box under the "Security" section.

## Cache Scavenger

The cache scavenger periodically cleans items from the cache. If an item is added to the cache and the cache is full, the item is added to the cache and the cache scavenger is scheduled. If the cache scavenger completes a round of scavenging and the cache has not reached the cache limit, the scavenger is scheduled for another round when another item is added to the cache. In general, the scavenger is scheduled when an added item puts the cache over its size limit. By default, the minimum time to live in the cache is set to 10 minutes unless otherwise specified in a cache-control directive. When the cache scavenger is initiated, there is no guarantee that the oldest items are the first to be deleted from the cache.

The cache is shared across all WinINet applications on the computer for the same user. Starting with Windows Vista and Windows Server 2008 the cache size is set to 1/32nd the size of the disk, with a minimum size of 8MB and a maximum size of 50MB.

## Using Flags to Control Caching

The caching flags allow an application to control when and how it uses the cache. These flags can be used alone or in combination with the *dwFlags* parameter in functions that access information or resources on the Internet. By default, the functions store all data downloaded from the Internet.

The following flags can be used to control caching.



| Value                                                                                                                                                  | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**INTERNET\_FLAG\_CACHE\_ASYNC**](api-flags.md#internet-flag-cache-async)                                                                            | This flag has no effect.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| [**INTERNET\_FLAG\_CACHE\_IF\_NET\_FAIL**](api-flags.md#internet-flag-cache-if-net-fail)                                                              | Returns the resource from the cache if the network request for the resource fails due to an [ERROR\_INTERNET\_CONNECTION\_RESET](wininet-errors.md#error-internet-connection-reset) or [ERROR\_INTERNET\_CANNOT\_CONNECT](wininet-errors.md#error-internet-cannot-connect) error. This flag is used by [**HttpOpenRequest**](httpopenrequest.md).                                                                                                              |
| [**INTERNET\_FLAG\_DONT\_CACHE**](api-flags.md#internet-flag-dont-cache)                                                                              | Does not cache the data, either locally or in any gateways. Identical to the preferred value, [**INTERNET\_FLAG\_NO\_CACHE\_WRITE**](api-flags.md#internet-flag-no-cache-write).                                                                                                                                                                                                                                                                                 |
|                                                                                                                                                        | Indicates that this is a Forms submission.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [**INTERNET\_FLAG\_FROM\_CACHE**](api-flags.md#internet-flag-from-cache)[**INTERNET\_FLAG\_FORMS\_SUBMIT**](api-flags.md#internet-flag-forms-submit) | Does not make network requests. All entities are returned from the cache. If the requested item is not in the cache, a suitable error, such as ERROR\_FILE\_NOT\_FOUND, is returned. Only the [**InternetOpen**](internetopen.md) function uses this flag.                                                                                                                                                                                                       |
| [**INTERNET\_FLAG\_FWD\_BACK**](api-flags.md#internet-flag-fwd-back)                                                                                  | Indicates that the function should use the copy of the resource that is currently in the Internet cache. The expiration date and other information about the resource is not checked. If the requested item is not found in the Internet cache, the system attempts to locate the resource on the network. This value was introduced in Microsoft Internet Explorer 5 and is associated with the **Forward** and **Back** button operations of Internet Explorer. |
| [**INTERNET\_FLAG\_HYPERLINK**](api-flags.md#internet-flag-hyperlink)                                                                                 | Forces the application to reload a resource if no expire time and no last-modified time were returned when the resource was stored in the cache.                                                                                                                                                                                                                                                                                                                  |
| [**INTERNET\_FLAG\_MAKE\_PERSISTENT**](api-flags.md#internet-flag-make-persistent)                                                                    | No longer supported.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| [**INTERNET\_FLAG\_MUST\_CACHE\_REQUEST**](api-flags.md#internet-flag-must-cache-request)                                                             | Causes a temporary file to be created if the file cannot be cached. This is identical to the preferred value, [**INTERNET\_FLAG\_NEED\_FILE**](api-flags.md#internet-flag-need-file).                                                                                                                                                                                                                                                                            |
| [**INTERNET\_FLAG\_NEED\_FILE**](api-flags.md#internet-flag-need-file)                                                                                | Causes a temporary file to be created if the file cannot be cached.                                                                                                                                                                                                                                                                                                                                                                                               |
| [**INTERNET\_FLAG\_NO\_CACHE\_WRITE**](api-flags.md#internet-flag-no-cache-write)                                                                     | Rejects any attempt by the function to store data downloaded from the Internet in the cache. This flag is necessary if the application does not want any downloaded resources to be stored locally.                                                                                                                                                                                                                                                               |
| [**INTERNET\_FLAG\_NO\_UI**](api-flags.md#internet-flag-no-ui)                                                                                        | Disables the cookie dialog box. This flag can be used by [**HttpOpenRequest**](httpopenrequest.md) and [**InternetOpenUrl**](internetopenurl.md) (HTTP requests only).                                                                                                                                                                                                                                                                                          |
| [**INTERNET\_FLAG\_OFFLINE**](api-flags.md#internet-flag-offline)                                                                                     | Prevents the application from sending requests to the network. All requests are resolved using the resources stored in the cache. If the resource is not in the cache, a suitable error, such as ERROR\_FILE\_NOT\_FOUND, is returned.                                                                                                                                                                                                                            |
| [**INTERNET\_FLAG\_PRAGMA\_NOCACHE**](api-flags.md#internet-flag-pragma-nocache)                                                                      | Forces the request to be resolved by the origin server, even if a cached copy exists on the proxy. The [**InternetOpenUrl**](internetopenurl.md) function (on HTTP and HTTPS requests only) and [**HttpOpenRequest**](httpopenrequest.md) function use this flag.                                                                                                                                                                                               |
| [**INTERNET\_FLAG\_RELOAD**](api-flags.md#internet-flag-reload)                                                                                       | Forces the function to retrieve the requested resource directly from the Internet. The information that is downloaded is stored in the cache.                                                                                                                                                                                                                                                                                                                     |
| [**INTERNET\_FLAG\_RESYNCHRONIZE**](api-flags.md#internet-flag-resynchronize)                                                                         | Causes an application to perform a conditional download of the resource from the Internet. If the version stored in the cache is current, the information is downloaded from the cache. Otherwise, the information is reloaded from the server.                                                                                                                                                                                                                   |



 

## Persistent Caching Functions

Clients that need persistent caching services use the persistent caching functions to allow their applications to save data in the local file system for subsequent use, such as in situations where a low-bandwidth link limits access to the data, or the access is not available at all.

The cache functions provide persistent caching and offline browsing. Unless the [**INTERNET\_FLAG\_NO\_CACHE\_WRITE**](api-flags.md#internet-flag-no-cache-write) flag explicitly specifies no caching, the functions cache all data downloaded from the network. The responses to POST data are not cached.

## Using the Persistent URL Cache Functions

The following persistent URL cache functions allow an application to access and manipulate information stored in the cache.



| Function                                                           | Description                                                                                                                                                   |
|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CommitUrlCacheEntryA**](commiturlcacheentrya.md)               | Caches data in the specified file in the cache storage and associates it with the given URL.                                                                  |
| [**CommitUrlCacheEntryW**](commiturlcacheentryw.md)               | Caches data in the specified file in the cache storage and associates it with the given URL.                                                                  |
| [**CreateUrlCacheEntry**](createurlcacheentry.md)                 | Allocates the requested cache storage and creates a local file name for saving the cache entry that corresponds to the source name.                           |
| [**CreateUrlCacheGroup**](createurlcachegroup.md)                 | Generates a cache group identification.                                                                                                                       |
| [**DeleteUrlCacheEntry**](deleteurlcacheentry.md)                 | Removes the file associated with the source name from the cache, if the file exists.                                                                          |
| [**DeleteUrlCacheGroup**](deleteurlcachegroup.md)                 | Releases a **GROUPID** and any associated state in the cache index file.                                                                                      |
| [**FindCloseUrlCache**](findcloseurlcache.md)                     | Closes the specified enumeration handle.                                                                                                                      |
| [**FindFirstUrlCacheEntry**](findfirsturlcacheentry.md)           | Begins the enumeration of the cache.                                                                                                                          |
| [**FindFirstUrlCacheEntryEx**](findfirsturlcacheentryex.md)       | Begins a filtered enumeration of the cache.                                                                                                                   |
| [**FindNextUrlCacheEntry**](findnexturlcacheentry.md)             | Retrieves the next entry in the cache.                                                                                                                        |
| [**FindNextUrlCacheEntryEx**](findnexturlcacheentryex.md)         | Retrieves the next entry in a filtered cache enumeration.                                                                                                     |
| [**GetUrlCacheEntryInfo**](geturlcacheentryinfo.md)               | Retrieves information about a cache entry.                                                                                                                    |
| [**GetUrlCacheEntryInfoEx**](geturlcacheentryinfoex.md)           | Searches for the URL after translating any cached redirections that would be applied in offline mode by [**HttpSendRequest**](httpsendrequest.md).           |
| [**ReadUrlCacheEntryStream**](readurlcacheentrystream.md)         | Reads the cached data from a stream that has been opened using [**RetrieveUrlCacheEntryStream**](retrieveurlcacheentrystream.md).                            |
| [**RetrieveUrlCacheEntryFile**](retrieveurlcacheentryfile.md)     | Retrieves a cache entry from the cache in the form of a file.                                                                                                 |
| [**RetrieveUrlCacheEntryStream**](retrieveurlcacheentrystream.md) | Provides the most efficient and implementation-independent way of accessing the cache data.                                                                   |
| [**SetUrlCacheEntryGroup**](seturlcacheentrygroup.md)             | Adds or removes entries from a cache group.                                                                                                                   |
| [**SetUrlCacheEntryInfo**](seturlcacheentryinfo.md)               | Sets the specified members of the [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure.                                                |
| [**UnlockUrlCacheEntryFile**](unlockurlcacheentryfile.md)         | Unlocks the cache entry that was locked when the file was retrieved for use from the cache by [**RetrieveUrlCacheEntryFile**](retrieveurlcacheentryfile.md). |
| [**UnlockUrlCacheEntryStream**](unlockurlcacheentrystream.md)     | Closes the stream that has been retrieved using [**RetrieveUrlCacheEntryStream**](retrieveurlcacheentrystream.md).                                           |



 

### Enumerating the Cache

The [**FindFirstUrlCacheEntry**](findfirsturlcacheentry.md) and [**FindNextUrlCacheEntry**](findnexturlcacheentry.md) functions enumerate the information stored in the cache. [**FindFirstUrlCacheEntry**](findfirsturlcacheentry.md) starts the enumeration by taking a search pattern, a buffer, and a buffer size to create a handle and return the first cache entry. [**FindNextUrlCacheEntry**](findnexturlcacheentry.md) takes the handle created by [**FindFirstUrlCacheEntry**](findfirsturlcacheentry.md), a buffer, and a buffer size to return the next cache entry.

Both functions store an [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure in the buffer. The size of this structure varies for each entry. If the buffer size passed to either function is insufficient, the function fails and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns ERROR\_INSUFFICIENT\_BUFFER. The buffer size variable contains the buffer size that was needed to retrieve that cache entry. A buffer of the size indicated by the buffer size variable should be allocated, and the function should be called again with the new buffer.

The [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure contains the structure size, URL of the cached information, local file name, cache entry type, use count, hit rate, size, last modified time, expiration, last access, last synchronized time, header information, header information size, and file name extension.

The [**FindFirstUrlCacheEntry**](findfirsturlcacheentry.md) function takes a search pattern, a buffer that stores the [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure, and the buffer size. Currently, only the default search pattern, which returns all cache entries, is implemented.

After the cache is enumerated, the application should call [**FindCloseUrlCache**](findcloseurlcache.md) to close the cache enumeration handle.

The following example displays each cache entry's URL in a list box, **IDC\_CacheList**. It uses MAX\_CACHE\_ENTRY\_INFO\_SIZE to initially allocate a buffer, since early versions of the WinINet API do not enumerate the cache properly otherwise. Later versions do enumerate the cache properly and there is no cache size limit. All applications that run on computers with the version of the WinINet API from Internet Explorer 4.0 must allocate a buffer of the required size. For more information, see [Using Buffers](appendix-b-using-buffers.md).


```C++
int WINAPI EnumerateCacheOld(HWND hX)
{
    DWORD dwEntrySize;
    LPINTERNET_CACHE_ENTRY_INFO lpCacheEntry;
    DWORD MAX_CACHE_ENTRY_INFO_SIZE = 4096;
    HANDLE hCacheDir;
    int nCount=0;

    SendDlgItemMessage(hX,IDC_CacheList,LB_RESETCONTENT,0,0);
    
    SetCursor(LoadCursor(NULL,IDC_WAIT));

    dwEntrySize = MAX_CACHE_ENTRY_INFO_SIZE;
    lpCacheEntry = (LPINTERNET_CACHE_ENTRY_INFO) new char[dwEntrySize];
    lpCacheEntry->dwStructSize = dwEntrySize;

again:

    hCacheDir = FindFirstUrlCacheEntry(NULL,
                                       lpCacheEntry,
                                       &amp;dwEntrySize);
    if (!hCacheDir)                                             
    {
        delete[]lpCacheEntry;
        switch(GetLastError())
        {
            case ERROR_NO_MORE_ITEMS: 
                TCHAR tempout[80];
                _stprintf_s(tempout, 
                            80,   
                            TEXT("The number of cache entries = %d \n"),
                            nCount);
                MessageBox(hX,tempout,TEXT("Cache Enumeration"),MB_OK);
                FindCloseUrlCache(hCacheDir);
                SetCursor(LoadCursor(NULL,IDC_ARROW));
                return TRUE;
                break;
            case ERROR_INSUFFICIENT_BUFFER:
                lpCacheEntry = (LPINTERNET_CACHE_ENTRY_INFO) 
                                new char[dwEntrySize];
                lpCacheEntry->dwStructSize = dwEntrySize;
                goto again;
                break;
            default:
                ErrorOut( hX,GetLastError(),
                          TEXT("FindNextUrlCacheEntry Init"));
                FindCloseUrlCache(hCacheDir);
                SetCursor(LoadCursor(NULL,IDC_ARROW));
                return FALSE;
        }
    }

    SendDlgItemMessage(hX,IDC_CacheList,LB_ADDSTRING,
                       0,(LPARAM)(lpCacheEntry->lpszSourceUrlName));
    nCount++;
    delete (lpCacheEntry);

    do 
    {
        dwEntrySize = MAX_CACHE_ENTRY_INFO_SIZE;
        lpCacheEntry = (LPINTERNET_CACHE_ENTRY_INFO) new char[dwEntrySize];
        lpCacheEntry->dwStructSize = dwEntrySize;

retry:
        if (!FindNextUrlCacheEntry(hCacheDir,
                                   lpCacheEntry, 
                                   &amp;dwEntrySize))
        {
            delete[]lpCacheEntry;
            switch(GetLastError())
            {
                case ERROR_NO_MORE_ITEMS: 
                    TCHAR tempout[80];
                    _stprintf_s(tempout,
                                80,
                                TEXT("The number of cache entries = %d \n"),nCount);
                    MessageBox(hX,
                               tempout,
                               TEXT("Cache Enumeration"),MB_OK);
                    FindCloseUrlCache(hCacheDir);
                    return TRUE;
                    break;
                case ERROR_INSUFFICIENT_BUFFER:
                    lpCacheEntry = 
                             (LPINTERNET_CACHE_ENTRY_INFO) 
                              new char[dwEntrySize];
                    lpCacheEntry->dwStructSize = dwEntrySize;
                    goto retry;
                    break;
                default:
                    ErrorOut(hX,
                             GetLastError(),
                             TEXT("FindNextUrlCacheEntry Init"));
                    FindCloseUrlCache(hCacheDir);
                    return FALSE;
            }
        }

        SendDlgItemMessage(hX,
                           IDC_CacheList,LB_ADDSTRING,
                           0,
                          (LPARAM)(lpCacheEntry->lpszSourceUrlName));
        nCount++;
        delete[] lpCacheEntry;        
    }  while (TRUE);

    SetCursor(LoadCursor(NULL,IDC_ARROW));
    return TRUE;        
}
```



### Retrieving Cache Entry Information

The [**GetUrlCacheEntryInfo**](geturlcacheentryinfo.md) function lets you retrieve the [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure for the specified URL. This structure contains the structure size, URL of the cached information, local file name, cache entry type, use count, hit rate, size, last modified time, expiration, last access, last synchronized time, header information, header information size, and file name extension.

[**GetUrlCacheEntryInfo**](geturlcacheentryinfo.md) accepts a URL, a buffer for an [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure, and the buffer size. If the URL is found, the information is copied into the buffer. Otherwise, the function fails and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns ERROR\_FILE\_NOT\_FOUND. If the buffer size is insufficient to store the cache entry information, the function fails and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns ERROR\_INSUFFICIENT\_BUFFER. The size required to retrieve the information is stored in the buffer size variable.

[**GetUrlCacheEntryInfo**](geturlcacheentryinfo.md) does not do any URL parsing, so a URL that contains an anchor (\#) will not be found in the cache, even if the resource is cached. For example, if the URL "http://example.com/example.htm\#sample" is passed, the function returns ERROR\_FILE\_NOT\_FOUND even if "http://example.com/example.htm" is in the cache.

The following example retrieves the cache entry information for the specified URL. The function then displays the header information in the **IDC\_CacheDump** edit box.


```C++
int WINAPI GetCacheEntryInfo(HWND hX,LPTSTR lpszUrl)
{
    DWORD dwEntrySize=0;
    LPINTERNET_CACHE_ENTRY_INFO lpCacheEntry;

    SetCursor(LoadCursor(NULL,IDC_WAIT));
    if (!GetUrlCacheEntryInfo(lpszUrl,NULL,&amp;dwEntrySize))
    {
        if (GetLastError()!=ERROR_INSUFFICIENT_BUFFER)
        {
            ErrorOut(hX,GetLastError(),TEXT("GetUrlCacheEntryInfo"));
            SetCursor(LoadCursor(NULL,IDC_ARROW));
            return FALSE;
        }
        else
            lpCacheEntry = (LPINTERNET_CACHE_ENTRY_INFO) 
                            new char[dwEntrySize];
    }
    else
        return FALSE; // should not be successful w/ NULL buffer
                      // and 0 size

    if (!GetUrlCacheEntryInfo(lpszUrl,lpCacheEntry,&amp;dwEntrySize))
    {
        ErrorOut(hX,GetLastError(),TEXT("GetUrlCacheEntryInfo"));
        SetCursor(LoadCursor(NULL,IDC_ARROW));
        return FALSE;
    }
    else
    {
        if ((lpCacheEntry->dwHeaderInfoSize)!=0)
        {
            LPSTR(lpCacheEntry->lpHeaderInfo)
                                [lpCacheEntry->dwHeaderInfoSize]=TEXT('\0');
            SetDlgItemText(hX,IDC_Headers,
                           lpCacheEntry->lpHeaderInfo);
        }
        else
        {
            SetDlgItemText(hX,IDC_Headers,TEXT("None"));
        }

        SetCursor(LoadCursor(NULL,IDC_ARROW));
        return TRUE;
    }
        
}
```



### Creating a Cache Entry

An application uses the [**CreateUrlCacheEntry**](createurlcacheentry.md) and [**CommitUrlCacheEntry**](commiturlcacheentrya.md) functions to create a cache entry.

[**CreateUrlCacheEntry**](createurlcacheentry.md) accepts the URL, expected file size, and file name extension. The function then creates a local file name for saving the cache entry that corresponds to the URL and file name extension.

Using the local file name, write the data into the local file. After the data has been written to the local file, the application should call [**CommitUrlCacheEntry**](commiturlcacheentrya.md).

[**CommitUrlCacheEntry**](commiturlcacheentrya.md) accepts the URL, local file name, expiration, last modified time, cache entry type, header information, header information size, and file name extension. The function then caches data in the file specified in the cache storage and associates it with the given URL.

The following example uses the local file name, created by a previous call to [**CreateUrlCacheEntry**](createurlcacheentry.md), stored in the text box, **IDC\_LocalFile**, to store the text from the text box, **IDC\_CacheDump**, in the cache entry. After the data has been written to the file using **fopen**, **fprintf**, and **fclose**, the entry is committed using [**CommitUrlCacheEntry**](commiturlcacheentrya.md).


```C++
int WINAPI CommitEntry(HWND hX)
{
    LPTSTR lpszUrl, lpszExt, lpszFileName;
    LPTSTR lpszData,lpszSize;
    DWORD dwSize;
    DWORD dwEntryType=0;
    FILE *lpfCacheEntry;
    LPFILETIME lpdtmExpire, lpdtmLastModified;
    LPSYSTEMTIME lpdtmSysTime;
    errno_t err;

    if( SendDlgItemMessage(hX,IDC_RBNormal,BM_GETCHECK,0,0) )
    {
        dwEntryType = dwEntryType + NORMAL_CACHE_ENTRY;
    }
    else if( SendDlgItemMessage(hX,IDC_RBSticky, BM_GETCHECK,0,0) )
    {
        dwEntryType = dwEntryType + STICKY_CACHE_ENTRY;
    }
    else if(SendDlgItemMessage( hX,IDC_RBSparse, BM_GETCHECK,0,0) )
    {
        dwEntryType = dwEntryType + SPARSE_CACHE_ENTRY;
    }
 

    if( SendDlgItemMessage(hX,IDC_RBCookie, BM_GETCHECK,0,0))
    {
            dwEntryType = dwEntryType + COOKIE_CACHE_ENTRY;
    }
    else if( SendDlgItemMessage(hX,IDC_RBUrl, BM_GETCHECK,0,0) )
    {
        dwEntryType = dwEntryType + URLHISTORY_CACHE_ENTRY;
    }


    if( SendDlgItemMessage(hX,IDC_RBNone, BM_GETCHECK,0,0) )
    {
        dwEntryType=0;
    }
        
    lpdtmSysTime = new SYSTEMTIME;
    lpdtmExpire = new FILETIME;
    lpdtmLastModified = new FILETIME;

    GetLocalTime(lpdtmSysTime);
    SystemTimeToFileTime(lpdtmSysTime,lpdtmExpire);
    SystemTimeToFileTime(lpdtmSysTime,lpdtmLastModified);
    delete(lpdtmSysTime);

    lpszUrl = new TCHAR[MAX_PATH];
    lpszFileName = new TCHAR[MAX_PATH];
    lpszExt = new TCHAR[5];
    lpszSize = new TCHAR[10];

    GetDlgItemText(hX,IDC_SourceURL,lpszUrl,MAX_PATH);
    GetDlgItemText(hX,IDC_LocalFile,lpszFileName,MAX_PATH);
    GetDlgItemText(hX,IDC_FileExt,lpszExt,5);

    GetDlgItemText(hX,IDC_SizeLow,lpszSize,10);
    dwSize = (DWORD)_ttol(lpszSize);
    delete(lpszSize);

    if (dwSize==0)
    {
        if((MessageBox(hX,
                       TEXT("Incorrect File Size.\nUsing 8000 characters, Okay?\n"),
                       TEXT("Commit Entry"),MB_YESNO))
                        ==IDYES)
        {
            dwSize = 8000;
        }
        else
        {
            return FALSE;
        }
    }

    lpszData = new TCHAR[dwSize];
    GetDlgItemText(hX,IDC_CacheDump,lpszData,dwSize);
        
     err = _tfopen_s(&amp;lpfCacheEntry,lpszFileName,_T("w"));
     if (err)
        return FALSE;
    fprintf(lpfCacheEntry,"%s",lpszData);
    fclose(lpfCacheEntry);
    delete(lpszData);

    if ( !CommitUrlCacheEntry( lpszUrl, 
                               lpszFileName, 
                               *lpdtmExpire,
                               *lpdtmLastModified, 
                               dwEntryType,
                               NULL,
                               0,
                               lpszExt,
                               0) )
    {
        ErrorOut(hX,GetLastError(),TEXT("Commit Cache Entry"));
        delete(lpszUrl);
        delete(lpszFileName);
        delete(lpszExt);
        delete(lpdtmExpire);
        delete(lpdtmLastModified);
        return FALSE;
    }
    else
    {
        delete(lpszUrl);
        delete(lpszFileName);
        delete(lpszExt);
        delete(lpdtmExpire);
        delete(lpdtmLastModified);
        return TRUE;
    }
}
```



### Deleting a Cache Entry

The [**DeleteUrlCacheEntry**](deleteurlcacheentry.md) function takes a URL and removes the cache file associated with it. If the cache file does not exist, the function fails and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns ERROR\_FILE\_NOT\_FOUND. If the cache file is currently locked or in use, the function fails and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns ERROR\_ACCESS\_DENIED. The file is deleted when unlocked.

### Retrieving Cache Entry Files

For applications that require the file name of a resource, use the [**RetrieveUrlCacheEntryFile**](retrieveurlcacheentryfile.md) and [**UnlockUrlCacheEntryFile**](unlockurlcacheentryfile.md) functions. Applications that do not require the file name should use the [**RetrieveUrlCacheEntryStream**](retrieveurlcacheentrystream.md), [**ReadUrlCacheEntryStream**](readurlcacheentrystream.md), and [**UnlockUrlCacheEntryStream**](unlockurlcacheentrystream.md) functions to retrieve the information in the cache.

[**RetrieveUrlCacheEntryStream**](retrieveurlcacheentrystream.md) does not do any URL parsing, so a URL that contains an anchor (\#) will not be found in the cache, even if the resource is cached. For example, if the URL "http://example.com/example.htm\#sample" is passed, the function returns ERROR\_FILE\_NOT\_FOUND even if "http://example.com/example.htm" is in the cache.

[**RetrieveUrlCacheEntryFile**](retrieveurlcacheentryfile.md) accepts a URL, a buffer that stores the [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure, and the buffer size. The function is retrieved and locked for the caller.

After the information in the file has been used, the application should call [**UnlockUrlCacheEntryFile**](unlockurlcacheentryfile.md) to unlock the file.

### Cache Groups

To create a cache group, the [**CreateUrlCacheGroup**](createurlcachegroup.md) function must be called to generate a **GROUPID** for the cache group. Entries can be added to the cache group by supplying the cache entry's URL and the INTERNET\_CACHE\_GROUP\_ADD flag to the [**SetUrlCacheEntryGroup**](seturlcacheentrygroup.md) function. To remove a cache entry from a group, pass the cache entry's URL and the INTERNET\_CACHE\_GROUP\_REMOVE flag to [**SetUrlCacheEntryGroup**](seturlcacheentrygroup.md).

The [**FindFirstUrlCacheEntryEx**](findfirsturlcacheentryex.md) and [**FindNextUrlCacheEntryEx**](findnexturlcacheentryex.md) functions can be used to enumerate the entries in a specified cache group. After the enumeration is complete, the function should call [**FindCloseUrlCache**](findcloseurlcache.md).

## Handling Structures with Variable Size Information

The cache can contain variable size information for each URL stored. This is reflected in the [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) structure. When the cache functions return this structure, they create a buffer that is always the size of [**INTERNET\_CACHE\_ENTRY\_INFO**](internet-cache-entry-info.md) plus any variable size information. If a pointer member is not **NULL**, it points to the memory area immediately after the structure. While copying the buffer returned by a function into another buffer, the pointer members should be fixed to point to the appropriate place in the new buffer, as the following example shows.

``` syntax
lpDstCEInfo->lpszSourceUrlName = 
    (LPINTERNET_CACHE_ENTRY_INFO) ((LPBYTE) lpSrcCEInfo + 
       ((DWORD)(lpOldCEInfo->lpszSourceUrlName) - (DWORD)lpOldCEInfo));
```

Some cache functions fail with the ERROR\_INSUFFICIENT\_BUFFER error message if you specify a buffer that is too small to contain the cache entry information retrieved by the function. In this case, the function also returns the required size of the buffer. You can then allocate a buffer of the appropriate size and call the function again.

> [!Note]  
> WinINet does not support server implementations. In addition, it should not be used from a service. For server implementations or services use [Microsoft Windows HTTP Services (WinHTTP)](https://msdn.microsoft.com/library/windows/desktop/aa384273).

 

 

 



