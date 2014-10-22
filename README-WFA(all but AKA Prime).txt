hostapd as a RADIUS authentication server for WFA
=================================================

version 0.6.6 2008-11-28

Copyright (c) 2003-2008, Jouni Malinen <j@w1.fi> and contributors
All Rights Reserved.

This program is dual-licensed under both the GPL version 2 and BSD
license. Either license may be used at your option.

This product includes software developed by the OpenSSL Project
for use in the OpenSSL Toolkit (http://www.openssl.org/). This
product includes cryptographic software written by Eric Young
(eay@cryptsoft.com).


Introduction
------------

This directory includes a binary build of hostapd for 32-bit x86 Linux
systems. It is build using the build.config-wfa build time
configuration (.config file in hostapd build system) to include RADIUS
server and EAP server support needed for WFA testing program. It is
linked statically against OpenSSL 0.9.8i with a session ticket
override patch to enable EAP-FAST support. It is possible to build
hostapd for other CPUs if needed using the included configuration
file, but the official WFA testing is only validated with this binary
version.


Configuration
-------------

The configuration files included in this directory can be used to run
the tests in the WFA test plans. More information about configuration
files can be found from the hostapd source code package example files.

all-enabled.conf enables all EAP methods at the same time. It is
mainly for developer testing and the actual certification testing is
to be done with separate configuration files that enable only a single
EAP method to enforce that the correct method is used during the test
without having to verify this from any debug logs.

aka.conf - EAP-AKA
fast-gtc.conf - EAP-FAST/EAP-GTC with authenticated provisioning
fast-gtc-unknown-server-cert.conf - EAP-FAST/EAP-GTC with unknown server
	certificate (for negative STAUT test)
fast-mschapv2.conf - EAP-FAST/EAP-MSCHAPv2 with authenticated provisioning
peapv0.conf - EAP-PEAPv0/EAP-MSCHAPv2
peapv1.conf - EAP-PEAPv1/EAP-GTC
sim.conf - EAP-SIM
tls.conf - EAP-TLS
ttls.conf - EAP-TTLS/MSCHAPv2


Running
-------

hostapd authentication server includes two programs: hostapd is the
main RADIUS authentication server, including EAP server
implementation, hlr_auc_gw is a software simulator for providing GSM
and UMTS authentication data for EAP-SIM and EAP-AKA.

hlr_auc_gw is started first with the following command:

./hlr_auc_gw -g hlr_auc_gw.sim_db -m hlr_auc_gw.milenage_db 

hostapd is then started with the following command:

./hostapd all-enabled.conf

(or any other configuration file depending on the test case)

If debug output is needed to analyze authentication process, this can
be enabled by adding -ddK on the hostapd command line (e.g., ./hostapd
-ddK all-enabled.conf).


Special test procedures
-----------------------

SQN resynchronization for EAP-AKA USIM can be tested by stopping
(ctrl-c) and re-starting hlr_auc_gw. This clears the SQN back to their
initial values as specified in hlr_auc_gw.milenage_db.
