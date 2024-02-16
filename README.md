# Streamtape-Video-Downloader

## Simple snippet of code to get direct download from streamtape's link

i made this code to help anyone who wants to download or embed a video from a streamtape 
without having to go through their stupid site.

```java
  private static String SteamtapeGetDlLink(String link) {
    try {
      if (link.contains("/e/"))
        link = link.replace("/e/", "/v/"); 
      Document doc = Jsoup.connect(link).get();
      String htmlSource = doc.html();
      Pattern norobotLinkPattern = Pattern.compile("document\\.getElementById\\('norobotlink'\\)\\.innerHTML = (.+);");
      Matcher norobotLinkMatcher = norobotLinkPattern.matcher(htmlSource);
      if (norobotLinkMatcher.find()) {
        String norobotLinkContent = norobotLinkMatcher.group(1);
        Pattern tokenPattern = Pattern.compile("token=([^&']+)");
        Matcher tokenMatcher = tokenPattern.matcher(norobotLinkContent);
        if (tokenMatcher.find()) {
          String token = tokenMatcher.group(1);
          Elements divElements = doc.select("div#ideoooolink[style=display:none;]");
          if (!divElements.isEmpty()) {
            String streamtape = ((Element)Objects.<Element>requireNonNull(divElements.first())).text();
            String fullUrl = "https:/" + streamtape + "&token=" + token;
            return fullUrl + "&dl=1s";
          } 
        } 
      } 
    } catch (Exception exception) {}
    return null;
  }
  ```

 ### hope this can help you too, dont forget to credit me 😉
