aSmack - buildsystem for Smack on Android
=========================================

*This repository doesn't contain much code, it's a build environment!*

Tracking trunk can be hard. Doing massive changes on top of trunk can be
near impossible. We are mixing 6 open source projects to provide a working
xmpp library for Android. All trunk-based.

This repository contains a source fetching, patching and building script.
As well as all the minor changes to make an Android version fly.
See the patches/ folder for a detailed list of changes and scripts.

Compiled JARs
=============
Can be found here: https://github.com/Flowdalic/asmack/downloads
But be aware, they may be *outdated!*

Compiling aSmack
================

1. copy local.properties.example to local.properties and set the Android SDK path (e.g. sdk-location=/opt/android-sdk-update-manager/ on a gentoo system)

2. Run build.bash

Apps that use this fork of aSmack
=================================
- GTalkSMS ( http://code.google.com/p/gtalksms/ ) uses many features of Smack and XMPP on Android:
    - File Transfer
    - DNS SRV
    - MUC
    - Entity Caps
    just to name a few. 

- yaxim ( https://github.com/ge0rg/yaxim )
- your app?

ProviderManager
===============

In order to work correctly on Android, you need to register the Providers manually before you doing any XMPP activty. E.g. like this:

    private static void configure(ProviderManager pm) {
        //  Private Data Storage
        pm.addIQProvider("query","jabber:iq:private", new PrivateDataManager.PrivateDataIQProvider());
 
        //  Time
        try {
            pm.addIQProvider("query","jabber:iq:time", Class.forName("org.jivesoftware.smackx.packet.Time"));
        } catch (ClassNotFoundException e) {
            Log.w("Can't load class for org.jivesoftware.smackx.packet.Time");
        }
 
        //  XHTML
        pm.addExtensionProvider("html","http://jabber.org/protocol/xhtml-im", new XHTMLExtensionProvider());

        //  Roster Exchange
        pm.addExtensionProvider("x","jabber:x:roster", new RosterExchangeProvider());
        //  Message Events
        pm.addExtensionProvider("x","jabber:x:event", new MessageEventProvider());
        //  Chat State
        pm.addExtensionProvider("active","http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("composing","http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("paused","http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("inactive","http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        pm.addExtensionProvider("gone","http://jabber.org/protocol/chatstates", new ChatStateExtension.Provider());
        
        //   FileTransfer
        pm.addIQProvider("si","http://jabber.org/protocol/si", new StreamInitiationProvider());
        pm.addIQProvider("query","http://jabber.org/protocol/bytestreams", new BytestreamsProvider());
        pm.addIQProvider("open","http://jabber.org/protocol/ibb", new OpenIQProvider());
        pm.addIQProvider("close","http://jabber.org/protocol/ibb", new CloseIQProvider());
        pm.addExtensionProvider("data","http://jabber.org/protocol/ibb", new DataPacketProvider());
        
        //  Group Chat Invitations
        pm.addExtensionProvider("x","jabber:x:conference", new GroupChatInvitation.Provider());
        //  Service Discovery # Items    
        pm.addIQProvider("query","http://jabber.org/protocol/disco#items", new DiscoverItemsProvider());
        //  Service Discovery # Info
        pm.addIQProvider("query","http://jabber.org/protocol/disco#info", new DiscoverInfoProvider());
        //  Data Forms
        pm.addExtensionProvider("x","jabber:x:data", new DataFormProvider());
        //  MUC User
        pm.addExtensionProvider("x","http://jabber.org/protocol/muc#user", new MUCUserProvider());
        //  MUC Admin    
        pm.addIQProvider("query","http://jabber.org/protocol/muc#admin", new MUCAdminProvider());
        //  MUC Owner    
        pm.addIQProvider("query","http://jabber.org/protocol/muc#owner", new MUCOwnerProvider());
        //  Delayed Delivery
        pm.addExtensionProvider("x","jabber:x:delay", new DelayInformationProvider());
        //  Version
        try {
            pm.addIQProvider("query","jabber:iq:version", Class.forName("org.jivesoftware.smackx.packet.Version"));
        } catch (ClassNotFoundException e) {
            Log.w("Can't load class for org.jivesoftware.smackx.packet.Version");
        }
        //  VCard
        pm.addIQProvider("vCard","vcard-temp", new VCardProvider());
        //  Offline Message Requests
        pm.addIQProvider("offline","http://jabber.org/protocol/offline", new OfflineMessageRequest.Provider());
        //  Offline Message Indicator
        pm.addExtensionProvider("offline","http://jabber.org/protocol/offline", new OfflineMessageInfo.Provider());
        //  Last Activity
        pm.addIQProvider("query","jabber:iq:last", new LastActivity.Provider());
        //  User Search
        pm.addIQProvider("query","jabber:iq:search", new UserSearch.Provider());
        //  SharedGroupsInfo
        pm.addIQProvider("sharedgroup","http://www.jivesoftware.org/protocol/sharedgroup", new SharedGroupsInfo.Provider());
        //  JEP-33: Extended Stanza Addressing
        pm.addExtensionProvider("addresses","http://jabber.org/protocol/address", new MultipleAddressesProvider());
    }

Contribution
============

The easiest way to contribute is fork & pull request. You may also ask about
direct project access. Mind that minor changes can be applied to the smack
repository. You'll most likly want to help out on smack, not on aSmack.

Contributors
============

We do not keep a seperate CONTRIBUTORS file, and we discourage @author tags.
However you're free to add your full name to every git commit, and we will
preserver this. Let us know if you've helped on non-technical stuff and we'll
find a way to give you the deserved credit.

Reporting Problems / Debugging
==============================

We always provide source zips. Attach them to the jar in your favorite IDE.
Enable debugging mode (BOSH/XMPPConnection.DEBUG and config.setDebug)
Record a logcat
Remove your credentials (usually a base64 block inside <auth></auth>)

Your issue should contain
1. a logcat
2. a server to reproduce
3. the code you are using (for FOSS project we'll accept reposituroy URLs)

There is no guarantee that we will reply immediatly. But we will try to
investigate the problem.


Licences / Used libraries
=========================

We only accept Apache and BSD-like licences.
We are currently using code from

 * Apache Harmony (sasl/xml) (Apache Licence)
 * smack (xmpp) (Apache Licence)
 * novell-openldap-jldap (sasl) ( [OpenLDAP Licence][1] )
 * Apache qpid (sasl) (Apache Licence)
 * jbosh (BOSH) (Apache Licence)
 * dnsjava (dns srv lookups) (BSD)
 * custom code (various glue stuff) (WTFPL | BSD | Apache)

This should work for just about every project. Contact us if you have problems
with the licence.

  [1]: http://www.openldap.org/devel/cvsweb.cgi/~checkout~/LICENSE?rev=1.23.2.1&hideattic=1&sortbydate=0  "OpenLDAP Licence"
