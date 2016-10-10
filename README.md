# Spammy Placements Adwords Script

Use this AdWords script to export Display campaign placements into a Google Sheet with the following criteria:
- Clicks > 10
- CTR > 20%

I've found this generally catches spammy Mobile Apps and other unsual and low performing placements.

Customize parameters as needed and replace "sheetUrl" with the URL of your selected Google Sheet.


```javascript
function main() {
  var sheetUrl  = '';
  var sheet     = SpreadsheetApp.openByUrl(sheetUrl);
  var getSheet  = sheet.getActiveSheet();
  var DisplayCampaigns = AdWordsApp.report("SELECT CampaignId,CampaignName FROM CAMPAIGN_PERFORMANCE_REPORT WHERE AdvertisingChannelType='DISPLAY' DURING LAST_30_DAYS").rows();
  while(DisplayCampaigns.hasNext()){
    var DisplayCampaign = DisplayCampaigns.next();
    var placements = AdWordsApp.report("SELECT Criteria,Clicks,Ctr,Impressions FROM PLACEMENT_PERFORMANCE_REPORT WHERE CampaignId='"+DisplayCampaign['CampaignId']+"' AND Clicks>=10 AND Ctr>'0.20' DURING_LAST_30_DAYS").rows();
    while(placements.hasNext()){
      var placement = placements.next();
      getSheet.appendRow([placement['Criteria'],placement['Impressions'],placement['Clicks'],placement['Ctr'],DisplayCampaign['CampaignName']]);
    }
  }
}
```
