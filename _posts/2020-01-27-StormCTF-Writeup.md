---
layout: post
title: StormCTF Shmoocon Competition Write-up
---

## Spoiler Alert - I didn't win

Like most of the information security community, and those adjacent to it, I was unable to get Shmoocon tickets the normal way. Posting on Twitter, regularly searching "shmoocon" on Twitter, and monitoring various slacks have all been unsucessful. 

When [@StormCTF](https://twitter.com/StormCTF) posted that they were running a competition to give away 2 tickets, I jumped in!  I'm writing this after the competition has closed, so screenshots may be a bit more sparse than I prefer.  

## Let's Find a Flag

StormCTF gave 7 days and a link.  The link was to a page with just a picture, their logo.

![LOGO](https://raw.githubusercontent.com/marcslaughter/marcslaughter.github.io/master/images/89e66322d99a33cbabf35d1245eb5c5582c025694506503bea3f43aef25caad1.png)

Here's where I found my first red herring.  Note the file name.  But first, if you havn't heard of [Cyber Chef](https://gchq.github.io/CyberChef/), you are missing out.  Cyber Chef is a fully in-browser amalgamation of information manipulation tools.  It's hashing, obfuscating, compressing, stegonographying (that's not a word), extracting, reversing, etc. all in one glorious place. Depending on the content, one could complete an entire CTF, just using Cyber Chef.  After we have the logo, let's do just that.

The first step in solving this puzzle is to spend about 2 hours trying to manipulate that file name into something useful and relevant, then check twitter and see that StormCTF posted a [hint](https://twitter.com/StormCTF/status/1219431953289613313).

Ok, there's a pastebin link somewhere in this mess.  I opened the imagefile in notepad and Ctrl-F'd "paste" and found it.

![TextLogo](https://raw.githubusercontent.com/marcslaughter/marcslaughter.github.io/master/images/textlogo.PNG)

Work, where I'm writing this, uderstandably blocks pastebin.  Fortunately for you, I was working on this CTF last week at work too, so I asked someone else to go to the link and email me the contents.

From here it's just a question of a combination of trial-and-error and recognizing data formats. Either you know what bsae64 or hex looks like or you guess until the output isn't gibberish.  Rather, it can and probably will be gibbersh, but what we're looking for is gibberish that can be ungibberified.  Importantly, there was one step where a '=' gave it away.  '=' is a placeholder for base64 encoded text, padding the length to ensure there is no extraneous data.  So one of the steps concluded with a string that *began* with '='.  Without knowing that '=' pads base64, I'm not sure how I would have come up with a need to reverse. 

[And here it is...](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)From_Hex('Auto')Reverse('Character')From_Base64('A-Za-z0-9%2B/%3D',true)From_Hex('Auto')From_Base64('A-Za-z0-9%2B/%3D',true)&input=TTJRMU1UTXlOR1F6TnpVeE16STBaRE0zTkRVMU5EUmxNemMxT1RaaE5HVXpOell6TkRRMFpUTTNORFUwTnpSbE16YzBOVFprTkdVek56UTFOVFEwWlRNM05HUTBORFJsTXpjMU5UUTNOR1V6TnpRMU16STBaVE0zTm1JMU5EUmxNemMwT1Raa05HVXpOelExTkRjMFpUTTNOak0wTkRSbE16YzBPVFUwTkdVek56UXhOMkUwWkRNM05URTBOelJsTXpjME5UTXlOR1V6TnpVeE5EYzBaVE0zTkRrME5EUmxNemMxT1RRME5HVXpOell6TkRRMFpUTTNORGsxTkRSbE16YzFPVFEwTkdVek56VTFORGMwWlRNM05URTJaRFJsTXpjMllqVTBOR1V6TnpSa05tUTBaVE0zTlRrMU5EUmxNemMyTXpVME5HVXpOelExTlRjMFpUTTNOVGswTkRSbE16YzBPVFUwTkdVek56VTFOVFEwWlRNM05UazBOelJsTXpjMU1UWmtOR1V6TnpZM05tRTBaVE0zTkRVek1qUmxNemMxTVRRM05HVXpOell6TkRRMFpUTTNOVFUwTnpSbE16YzFOVFUwTkdVek56UTVOVFEwWlRNM05qYzJZVFJsTXpjME9UVTBOR1V6TnpRMU5tUTBaVE0zTlRrME56UmxNemMyTXpkaE5HVXpOelppTkRRMFpUTTNOVEUwTkRSbE16YzFNVFEzTkdVek56WmlOMkUwWlRNM05qYzNZVFJrTXpjME9UZGhOR1F6TnpRNU5tRTBaVE0zTlRFM1lUUmxNemMyTnpaaE5HVXpOelE1TjJFMFpETTNOVFUxTkRSbE16YzJNemRoTkdRek56UTFOVGMwWlRNM05UVTBORFJsTXpjMU9UVTBOR1V6TnpVeE5EUTBaVE0zTkRVM1lUUmtNemMxTVRaa05HVXpOelJrTm1FMFpUTTNOVGszWVRSbE16YzBPVFUwTkdVek56UmtOMkUwWkRNM05UVTFORFJs)

![SOLUTION](https://raw.githubusercontent.com/marcslaughter/marcslaughter.github.io/master/images/Solution.PNG)

