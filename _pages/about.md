---
permalink: /about/
title: "About"
---
<script>
  // This demo is licensed under the GNU GPL.
</script>
<script type="text/javascript" src="../assets/js/mespeak/mespeak.js"></script>
<script type="text/javascript">
  meSpeak.loadVoice("nl.json");

  function loadVoice(id) {
    var fname="nl.json";
    meSpeak.loadVoice(fname, voiceLoaded);
  }

  function voiceLoaded(success, message) {
    if (success) {
      console.log("Voice loaded: "+message+".");
    }
    else {
      console.log("Failed to load a voice: "+message);
    }
  }
</script>

Hi I'm <phoneme alphabet="ipa" ph="/ʃuɝd/ ">Sjoerd</phoneme> <input type="button" value="&sung;" onclick="javascript:meSpeak.speak('Sjoerd', { amplitude: 100, wordgap: 0, pitch: 65, speed: 150, variant: 'f3' });return false" />. Blogger ;-)

{{ site.description }}

<div>
  <div style="font-size:14px">
  <b>#CloudProductivity</b> | #MicrosoftCloud <br />
  #ModernWorkplace | #SaaS | #iPaaS <br />
  <b>#Microsoft365</b> | <b>#AzureAD</b> <br />
  <b>#MicrosoftTeams</b> | <b>#SharePoint</b> | #OneDrive <br />
  <i>#Security</i> | <i>#Compliance</i> <br />
  <b>#AzureDevOps</b> | <b>#VisualStudioCode</b> | <b><code>#PowerShell</code></b> <br />
  #FOSS | #GNU/Linux | <code>#Python</code> <br />
  #StrategyAdvice | <i>#ITGovernance</i> | <code>#ProcessAutomation</code><br />
  <i>#InfoArchitectureDesign</i> | #Portal | #Intranet <br />
  <code>#PowerPlatform</code> | #Workflow | #Forms | <b>#PowerApps</b> | <b>#PowerAutomate</b> <br />
  #DocumentManagement | #EnterpriseSearch<br />
  #ECM | #WCM | #TaxonomyDevelopment
  </div>
</div>
