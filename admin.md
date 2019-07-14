# Administration

Whave two controllers up and running, make sure both are connected and can ping eachother. There is as previously discussed no ethernet backplane so you need to make sure they can do that or nothing will work.

## Model Determination

I have what I think is a 522 controller and a 511 controller. The reason I believe that is because looking at /proc/cpuinfo I see 1.18 on one and 1.2 on the other, with MR1750K card ID reported as 0801 and 0702 respectively. I would love to hear about more ways to figure out the generation of controllers.

## Sanity Checking
Sanity checking runs a series of tests to ensure sanity of the configuration. You can either run it locally only by passing -current, or on both controllers. Beware that running on both controllers is quite slow as the current controller will SSH into the other controller for every check, which can take some time.

Example output:

```
[Mon Mar 07 05:58:59] root@noname:~ # smsanity -nc   

Sanity Checker v0.30 invoked on c1 (noname)
------------------------------------------ Kona 0 --------- Kona 1 ---------
Checking free memory...................... FAILED!          Passed           
Verifying RW partitions................... FAILED!          Passed           
Verifying Kona replacement is enabled..... FAILED!          Passed           
Checking running processes................ FAILED!          Passed           
Checking disk space....................... FAILED!          Passed           
Checking SBR status....................... skipped          skipped          
Verifying four online DA partitions....... FAILED!          Passed           
Verifying certain files do not exist...... Passed           Passed           
Verifying that LCPSS is in Dual mode...... FAILED!          FAILED!          
Verifying no open hardware problems....... FAILED!          FAILED!          
Verifying no open software problems....... FAILED!          FAILED!          
Checking file permissions................. FAILED!          Passed           
Verifying no open cabling problems........ FAILED!          Passed           
Verifying no open data loss problems...... FAILED!          Passed           
Checking symbolic links................... FAILED!          Passed           
Checking number of IML retries............ FAILED!          Passed           
Verifying no CF R/W errors................ FAILED!          Passed           
Scanning ranks............................ FAILED!          Passed           
Checking serials in ncipl (strict)........ FAILED!          FAILED!          
Checking serials in ncipl (vote).......... skipped          skipped          
Checking PDM ISS consistency.............. skipped          skipped          
Checking PDM corruption................... skipped          Passed           
Checking Pulled out BANJO................. FAILED!          Passed           
----------------------------------------------------------------------------

(*) Detailed information about the failed checks:

  date: Failed to execute '' on Kona 0
  memory: Kona 0: Failed to execute 'cat /proc/meminfo'
  mount-rw: Kona 0: Failed to execute 'busybox mount'
  kona-replacement: Kona 0: Remote file /lic/sm/bin/ncutils.conf does not exist or is not readable
  processes: Kona 0: Failed to determine which processes are running
  diskspace: Kona 0: Failed to execute 'df -a'
  4-dapart: Kona 0: Failed to execute 'df -a'
  dual-lcpss: Kona 0: Failed to execute catreef status/opmode
  dual-lcpss: Kona 1: Bad LCPSS status 'Single Cluster Operational'
  hw-problems: Kona 0: Failed to execute /lic/sm/bin/rss_displayProblem
  hw-problems: Kona 1: Open problem of type 0 (hardware) found, id=2005-01-01-00.54.48.548951
  sw-problems: Kona 0: Failed to execute /lic/sm/bin/rss_displayProblem
  sw-problems: Kona 1: Open problem of type 1 (software) found, id=2016-07-21-19.12.06.936383
  permissions: Kona 0: Failed to execute 'stat -c '%a' /persist/scratch'
  cabling-problems: Kona 0: Failed to execute /lic/sm/bin/rss_displayProblem
  dataloss-problems: Kona 0: Failed to execute /lic/sm/bin/rss_displayProblem
  sym-links: Kona 0: Failed to execute 'ls -l /dc /linuxrc /lic /home/shark/log /home/shark/tmp /home/shark/statesave /home/shark/config'
  num-iml-retries: Kona 0: Remote file /home/shark/config/imlretry does not exist or is not readable
  cf-rw-errors: Kona 0: Failed to execute 'grep _intr /var/log/messages | grep -v "grep" | wc -l'
  scanrank: Kona 0: Failed to execute 'dacmd -x scanrank'
  ncipl-strict: C0 serial number was found in only 2 files: /dapart/s1/ncipl.da,/dapart/s3/ncipl.da
Box serial number was found in only 2 files: /dapart/s1/ncipl.da,/dapart/s3/ncipl.da
C1 serial number was found in only 2 files: /dapart/s1/ncipl.da,/dapart/s3/ncipl.da
  pulled_out_banjo: Kona 0: Failed to execute /lic/sm/bin/rss_displayProblem

(*) This situation is OK for the following scenarios:

  - Node config status report
  - Nonconcurrent code load (before quiesce)
  - Concurrent code load (before quiesce)
```

# Licensing
There are some files that are interesting for licensing and product enablement. I haven't looked too much into this yet as my array is not fully up and running, but expect this section to grow. If you're so inclined, the code that handles feature activation seems to be called libSm.so and comes with debugging symbols baked in.

Files that are critical to licensing that I know so far is /persist/etc/fea${CHASSIS_SERIAL}.bin, /persist/etc/nc_mnta.cfg, and persist/etc/ncipl.da. 
