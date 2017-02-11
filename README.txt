TA-Microsoft-Sysmon v5.0.0  
----------------------------	
	
	Original Author: Adrian Hall
	Current maintainers: Jim Apger, Dave Herrald, James Brosdsky 
	Contributors:	https://github.com/dstaulcu, https://github.com/MikeKemmerer
	Version/Date: 5.0.0 11/22/2016
	Sourcetype: XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
	Has index-time ops: false
	Input Requirements: Sysmon 3.1 or later installed with Splunk Universal Forwarder for Windows

Update History
----------------------------

  5.0.0
  --------
  Updates to support sysmon V5
  Note the sample configuration included below was modified to exclued the ImageLoad section which was found to be causing BSOD on some Windows 7 test systems.
  Special thanks to David Staulcup (https://github.com/dstaulcu) for ongoing assistance and contributions 

	4.0.0
	--------
	Minor updates to support Sysmon V4 and optimize the hash field extraction. 
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/8
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/9
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/10
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/11

	3.2.3
	--------
	Minor updates to add workflow actions via pull request and subsequent fine tuning.
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/5
	See: https://github.com/splunk/TA-microsoft-sysmon/pull/6

	3.2.2
	--------
	Minor updates to extract various hash values into individual fields for convenience:
	https://github.com/splunk/TA-microsoft-sysmon/issues/4

	3.2.1
	--------
	Minor updates to align with sysmon version 3.21. For details see:
	https://github.com/splunk/TA-microsoft-sysmon/issues/1
	https://github.com/splunk/TA-microsoft-sysmon/issues/2
	https://github.com/splunk/TA-microsoft-sysmon/issues/3

	3.1.1
	-------
	Major modification of the version to better align with SplunkBase.
	Fixed typos in eventtypes.conf and props.conf

	0.3.1
	-----
	Lookup table added to support Sysmon 3.1
	Additional CIM compliance added
	Example config added
	Revved to version 0.3.1 to match current Sysmon version


Using this TA
----------------------------

	Configuration: Install TA via GUI on all search heads, install
	via your preferred method (manual or Deployment Server) on
	forwarders running on Windows that have Sysmon 3.1 or greater
	installed

	Ensure that you have at least version 6.2.0 universal forwarders.
	This is because of the Windows XML event log format.

  http://blogs.splunk.com/2014/11/04/splunk-6-2-feature-overview-xml-event-logs/

	For additional info on Sysmon see here:

  http://blogs.splunk.com/2014/11/24/monitoring-network-traffic-with-sysmon-and-splunk/

Support
----------------------------

	This is a community supported TA. As such, post to answers.splunk.com
	and reference it. Someone should be with you shortly.

	Pull requests via github are welcome!

Sample Config
----------------------------

	Sysmon is capable of delivering a large amount of events into your
	Splunk instance. The following configuration, loaded into each
	system running Sysmon 3.1 or greater, will reduce the amount of data considerably.
	Special thanks go to Jeff Walzer from the University of Pittsburgh for
	originally helping to test this (walzer@pitt.edu).

	Load this via sysmon -c (filename) from an admin-level command prompt.
	(after you have placed it in a text file). You may get some 
	unusual errors - these are benign and can be ignored. Check the
	filtering via a "sysmon -c" with no argument.

	For additional Sysmon filtering, remove the entire ImageLoad 
	section.

**** CUT HERE ****

<Sysmon schemaversion="3.2">
  <HashAlgorithms>*</HashAlgorithms>
  <EventFiltering>
    <!-- Log all drivers except if the signature -->
    <!-- contains Microsoft or Windows -->
    <DriverLoad onmatch="exclude">
      <Signature condition="contains">microsoft</Signature>
      <Signature condition="contains">windows</Signature>
    </DriverLoad>
    <!-- Exclude certain processes that cause high event volumes -->
    <ProcessCreate onmatch="exclude">
      <Image condition="contains">splunk</Image>
      <Image condition="contains">streamfwd</Image>
      <Image condition="contains">splunkd</Image>
      <Image condition="contains">splunkD</Image>
      <Image condition="contains">splunk</Image>
      <Image condition="contains">splunk-optimize</Image>
      <Image condition="contains">splunk-MonitorNoHandle</Image>
      <Image condition="contains">splunk-admon</Image>
      <Image condition="contains">splunk-netmon</Image>
      <Image condition="contains">splunk-regmon</Image>
      <Image condition="contains">splunk-winprintmon</Image>
      <Image condition="contains">btool</Image>
      <Image condition="contains">PYTHON</Image>
    </ProcessCreate>
    <ProcessTerminate onmatch="exclude">
      <Image condition="contains">splunk</Image>
      <Image condition="contains">streamfwd</Image>
      <Image condition="contains">splunkd</Image>
      <Image condition="contains">splunkD</Image>
      <Image condition="contains">splunk</Image>
      <Image condition="contains">splunk-optimize</Image>
      <Image condition="contains">splunk-MonitorNoHandle</Image>
      <Image condition="contains">splunk-admon</Image>
      <Image condition="contains">splunk-netmon</Image>
      <Image condition="contains">splunk-regmon</Image>
      <Image condition="contains">splunk-winprintmon</Image>
      <Image condition="contains">btool</Image>
      <Image condition="contains">PYTHON</Image>
    </ProcessTerminate>
    <FileCreateTime onmatch="exclude">
      <Image condition="contains">splunk</Image>
      <Image condition="contains">streamfwd</Image>
      <Image condition="contains">splunkd</Image>
      <Image condition="contains">splunkD</Image>
      <Image condition="contains">splunk</Image>
      <Image condition="contains">splunk-optimize</Image>
      <Image condition="contains">splunk-MonitorNoHandle</Image>
      <Image condition="contains">splunk-admon</Image>
      <Image condition="contains">splunk-netmon</Image>
      <Image condition="contains">splunk-regmon</Image>
      <Image condition="contains">splunk-winprintmon</Image>
      <Image condition="contains">btool</Image>
      <Image condition="contains">PYTHON</Image>
    </FileCreateTime>
  </EventFiltering>
</Sysmon>


**** CUT HERE ****
