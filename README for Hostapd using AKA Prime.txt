Running Hostapd with simulator
-------

hostapd authentication server includes two programs: hostapd is the
main RADIUS authentication server, including EAP server
implementation, hlr_auc_gw is a software simulator for providing GSM
and UMTS authentication data for EAP-SIM and EAP-AKA.

hlr_auc_gw is started first with the following command:

./hlr_auc_gw -g hlr_auc_gw.sim_db -m hlr_auc_gw.milenage_db

Then, to run hostapd:

./hostapd aka_prime.conf

If debug output is needed to analyze authentication process, this can
be enabled by adding -ddK on the hostapd command line (e.g., ./hostapd
-ddK aka_prime.conf).

Please see 11n tech ops manual for more information
