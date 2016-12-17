This is an informal schema for an open source test suite for the [Authenticated Recieved Chain(ARC)](https://tools.ietf.org/html/draft-ietf-dmarc-arc-protocol-01) protocol, illustrated with examples.  This was prototyped from [the OpenSPF Test Suite](http://www.openspf.org/Test_Suite/Schema)

The syntax is YAML. The top level object is a "scenario". A file can consist of multiple scenarios separated by '---' on a line by itself. Lexical comments are introduced by '#' and continue to the end of a line. Lexical comments are ignored. There are also comment fields which are part of a scenario, and used for purposes such as automating the annotated RFC. Here are two sample scenarios:

'''
# Sample scenarios
description: >-
  dummy scenario
tests:
  test1:
    spec:        12/16
    description: basic test
    email:       >-
                 Return-Path: <jqd@d1.example>
                 Received: from [10.10.10.131] (w-x-y-z.dsl.static.isp.com [w.x.y.z])
                     (authenticated bits=0)
                     by segv.d1.example with ESMTP id t0FN4a8O084569;
                     Thu, 14 Jan 2015 15:00:01 -0800 (PST)
                     (envelope-from jqd@d1.example)
                 DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/simple; d=d1.example;
                     s=20130426; t=1421363082;
                     bh=EoJqaaRvhrngQxmQ3VnRIIMRBgecuKf1pdkxtfGyWaU=;
                     h=Message-ID:Date:From:MIME-Version:To:CC:Subject:Content-Type:
                      Content-Transfer-Encoding;
                     b=HxsvPubDE+R96v9dM9Y7V3dJUXvajd6rvF5ec5BPe/vpVBRJnD4I2weEIyYijrvQw
                      bv9uUA1t94kMN0Q+haFo6hiQPnkuDxku5+oxyZWOqtNH7CTMgcBWWTp4QD4Gd3TRJl
                      gotsX4RkbNcUhlfnoQ0p+CywWjieI8aR6eof6WDQ=
                 Message-ID: <54B84785.1060301@d1.example>
                 Date: Thu, 14 Jan 2015 15:00:01 -0800
                 From: John Q Doe <jqd@d1.example>
                 To: arc@dmarc.org
                 Subject: Example 1
                 Hey gang,
                 This is a test message.
                 --J.
	auth-res:    >-
	             lists.example.org;
                 spf=pass smtp.mfrom=jqd@d1.example;
                 dkim=pass (1024-bit key) header.i=@d1.example;
                 dmarc=pass
	AS:          >-
                 i=1; a=rsa-sha256; t=1421363107;
                 s=seal2015; d=example.org; cv=none;
                 b=pCw3Qxgfs9E1qnyNZ+cTTF3KHgAjWwZz++Rju0BceSiuwIg0Pkk+3RZH/kaiz61
                  TX6RVT6E4gs49Sstp41K7muj1OR5R6Q6llahLlQJZ/YfDZ3NImCU52gFWLUD7L69
                  EU8TzypfkUhscqXjOJgDwjIceBNNOfh3Jy+V8hQZrVFCw0A=
    AAR:         >-
	             i=1; lists.example.org;
                 spf=pass smtp.mfrom=jqd@d1.example;
                 dkim=pass (1024-bit key) header.i=@d1.example;
                 dmarc=pass
    AMS:         >-
	             i=1; a=rsa-sha256; c=relaxed/relaxed;
                 d=example.org; s=clochette; t=1421363105;
                 bh=FjQYm3HhXStuzauzV4Uc02o55EzATNfL4uBvEoy7k3s=;
                 h=List-Id:List-Unsubscribe:List-Archive:List-Post:
                  List-Help:List-Subscribe:Reply-To:DKIM-Signature;
                 b=Wb4EiVANwAX8obWwrRWpmlhxmdIvj0dv0psIkiaGOOug32iTAcc74/iWvlPXpF1F5
                  vYVF0mw5cmKOa824tKkUOOE3yinTAekqnly7GJuFCDeSA1fQHhStVV7BzAr3A+m4bw
                  a6RIDgr3rOPJil678dZTHfztFWyjwIUxB5Ajxj/M=
?domain:        example.org
?selector:      dummy
privatekey: >-

dkim-keys:
  dummy.example.org: >-

comment: >-
  This is a comment





questions:
auth res
dont care about domain or selector?
include headers?
timestamps






- well-formed messages
  - all inputs to arc-seal
  - all inputs to arc-message-signature
  - all inputs to arc-authentication-results

  - unsigned messages
  - 1 sig
  - > 1 sig

  - valid messages
  - invalid messages (+ all ways to fail)

  - various types of dns failures

- non-well formed messages messages



cover all the RFC keywords
cover other important aspects (syntax, tables, common mistakes, tricky corner cases, etc.)


DNS errors
When the last entry for a DNS name is the string TIMEOUT, the test driver should simulate a DNS timeout exception for queries that do not match any preceding records. When the last map for a DNS name is the map RCODE: n, the test driver should simulate a DNS error with code n. The test suite currently uses TIMEOUT, but not RCODE.




  def sign(self, selector, domain, privkey, auth_results, chain_validation_status,
        include_headers=None):



http://www.openspf.org/blobs/testspf.py
