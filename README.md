# Remove_YT_WatchLater
This repository contains a tool to help you manage and clean up your YouTube Watch Later playlist by automatically removing videos based on specified criteria.

# Load the 'Watch later' page
From your PC, open the Youtube 'Watch later' page

# Retrieve the list of 'setVideoId'
Open 'dev tools' of browser (by press 'F12' or 'Ctrl + Shift + I')
Then define the javascript function from the Console,

function extractSetVideoIds(data) {
  const videoIds = [];

  try {
    const tabs = data.twoColumnBrowseResultsRenderer.tabs;
    tabs.forEach(tab => {
      const sections = tab.tabRenderer.content.sectionListRenderer.contents;
      sections.forEach(section => {
        const items = section.itemSectionRenderer.contents;
        items.forEach(item => {
          const videos = item.playlistVideoListRenderer.contents;
          videos.forEach(video => {
            if (video.playlistVideoRenderer && video.playlistVideoRenderer.setVideoId) {
              videoIds.push({
                setVideoId: video.playlistVideoRenderer.setVideoId,
                action: "ACTION_REMOVE_VIDEO"
              });
            }
          });
        });
      });
    });
  } catch (error) {
    console.error('Error parsing data:', error);
  }

  return videoIds;
}

-----------------------

# Get a recent CURL of a remove action
right click on the 'edit_playlist?prettyPrint=false' request
 -> Copy
 -> Copy as cURL (bash)

 Here is an example, 
 curl 'https://www.youtube.com/youtubei/v1/browse/edit_playlist?prettyPrint=false' \
  -H 'accept: */*' \
  -H 'accept-language: zh-CN,zh;q=0.9' \
  -H 'authorization: ' \
  -H 'content-type: application/json' \
  -H 'cookie: <cookie> \
  -H 'origin: https://www.youtube.com' \
  -H 'priority: u=1, i' \
  -H 'referer: https://www.youtube.com/playlist?list=WL' \
  -H 'sec-ch-ua: "Not)A;Brand";v="99", "Google Chrome";v="127", "Chromium";v="127"' \
  -H 'sec-ch-ua-arch: "x86"' \
  -H 'sec-ch-ua-bitness: "64"' \
  -H 'sec-ch-ua-form-factors: "Desktop"' \
  -H 'sec-ch-ua-full-version: "127.0.6533.100"' \
  -H 'sec-ch-ua-full-version-list: "Not)A;Brand";v="99.0.0.0", "Google Chrome";v="127.0.6533.100", "Chromium";v="127.0.6533.100"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-model: ""' \
  -H 'sec-ch-ua-platform: "Windows"' \
  -H 'sec-ch-ua-platform-version: "10.0.0"' \
  -H 'sec-ch-ua-wow64: ?0' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: same-origin' \
  -H 'sec-fetch-site: same-origin' \
  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36' \
  -H 'x-client-data: ' \
  -H 'x-goog-authuser: 1' \
  -H 'x-goog-visitor-id: ' \
  -H 'x-origin: https://www.youtube.com' \
  -H 'x-youtube-bootstrap-logged-in: true' \
  -H 'x-youtube-client-name: 1' \
  -H 'x-youtube-client-version: 2.20240808.00.00' \
  --data-raw '{"context":{"client":{"hl":"zh-CN","gl":"FR","remoteHost":"81.65.162.131","deviceMake":"","deviceModel":"","visitorData":"","userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36,gzip(gfe)","clientName":"WEB","clientVersion":"2.20240808.00.00","osName":"Windows","osVersion":"10.0","originalUrl":"https://www.youtube.com/playlist?list=WL","platform":"DESKTOP","clientFormFactor":"UNKNOWN_FORM_FACTOR","configInfo":{"appInstallData":""},"userInterfaceTheme":"USER_INTERFACE_THEME_LIGHT","timeZone":"Europe/Paris","browserName":"Chrome","browserVersion":"127.0.0.0","acceptHeader":"text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7","deviceExperimentId":"","screenWidthPoints":1029,"screenHeightPoints":953,"screenPixelDensity":1,"screenDensityFloat":1,"utcOffsetMinutes":120,"connectionType":"CONN_CELLULAR_4G","memoryTotalKbytes":"8000000","mainAppWebInfo":{"graftUrl":"https://www.youtube.com/playlist?list=WL","pwaInstallabilityStatus":"PWA_INSTALLABILITY_STATUS_CAN_BE_INSTALLED","webDisplayMode":"WEB_DISPLAY_MODE_BROWSER","isWebNativeShareAvailable":true}},"user":{"lockedSafetyMode":false},"request":{"useSsl":true,"consistencyTokenJars": ... ,"internalExperimentFlags":[]},"clickTracking":{"clickTrackingParams":"COkBENOuBBgIIhMIqreqsoLphwMV6thJBx1pzQBs"},"adSignalsInfo":{"params":[{"key":"dt","value":"1723244602359"},{"key":"flash","value":"0"},{"key":"frm","value":"0"},{"key":"u_tz","value":"120"},{"key":"u_his","value":"12"},{"key":"u_h","value":"1080"},{"key":"u_w","value":"1920"},{"key":"u_ah","value":"1040"},{"key":"u_aw","value":"1920"},{"key":"u_cd","value":"24"},{"key":"bc","value":"31"},{"key":"bih","value":"953"},{"key":"biw","value":"1012"},{"key":"brdim","value":"0,0,0,0,1920,0,1920,1040,1029,953"},{"key":"vis","value":"1"},{"key":"wgl","value":"true"},{"key":"ca_type","value":"image"}],"bid":"ANyPxKqLbd2u8tJqNxGK-XkvWmYP92vOJPTOoDU96hFo63K_z0jP0q7qggfM-rIaXVgVA6ZzSsMelEQVN42FXUlgFOB-P8eVpQ"}},"actions":[{"setVideoId":"5FC1E3418B9A81AE","action":"ACTION_REMOVE_VIDEO"}],"params":"CAFAAQ%3D%3D","playlistId":"WL"}'

# complete the videoId list
- Back to Console, then launch js

JSON.stringify(extractSetVideoIds(ytInitialData.contents))

- Copy the content
- Replace the array of videoId by the copied content

# launche the cURL 
- by Postman or by Powershell


