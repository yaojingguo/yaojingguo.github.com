---
layout: post
title: "Linux Kernel Time"
description: ""
category:
tags: []
---
{% include JB/setup %}

## Hardware Timers
Provide wall time:

- Real Time Clock (RTC)

Global timer in preference:

- ACPI Power Managegment Timer (ACPI PMT): embeded clock signal at 3.58 MHZ
- High Precision Event Timer (HPET)
- Time Stamp Counter (TSC): external clock signal, varied frequency, with a high
	frequency.
- Programmable Interval Timer (PIT): embedded clock signal

`jiffies` is updated by global timer. `xtime` is initialized from RTC.
`jiffies` store the time passed since OS started. It is used by all the processors.
Global timer is used to update `jiffies`. Local timer is used to update
process-load usage account and drive preemption.

Local timer device:
- APIC local timer

For UP machine, global timer interrupt calls `update_process_times`. For SMP
machine, local timer interrupts call `update_process_times`.

`update_process_times` calls `scheduler_tick` which calls `task_tick`. For CFS
schedule class, it is `task_tick_fair` which callls` entity_tick`. `entity_tick` calls
`update_curr `and `resched_task`.


Function invocation chain:

```
resched_task --> set_tsk_need_resched.
try_to_wake_up --> check_preempt_curr --> check_preempt_wakeup.

do_timer
  tick_periodic
    tick_handle_periodic
  tick_do_update_jiffies64
    tick_sched_do_timer
  xtime_update

apic_timer_interrupt
  smp_apic_timer_interrupt
```

On a SMP machine with local APIC, global timer is only used during boot time.
After local APIC is setup up, local timer interrupts both call
`update_process_times` and update `jiffies`. The global timer is unused. All CPUs
perform `update_process_times`. But only one CPU updates `jiffies`.

The result of `cat /proc/timer_list` on a 64 bit ubuntu 14.04 machine. Kernel
version is 3.13.

```
Timer List Version: v0.7
HRTIMER_MAX_CLOCK_BASES: 4
now at 4247480622153 nsecs

cpu: 0
 clock 0:
  .base:       0000000000000000
  .index:      0
  .resolution: 1 nsecs
  .get_time:   ktime_get
  .offset:     0 nsecs
active timers:
 #0: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, /usr/bin/termin/2683
 # expires at 4247488930657-4247488980657 nsecs [in 8308504 to 8358504 nsecs]
 #1: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2195
 # expires at 4247489179536-4247489229536 nsecs [in 8557383 to 8607383 nsecs]
 #2: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, chrome/3286
 # expires at 4247753891896-4247754391895 nsecs [in 273269743 to 273769742 nsecs]
 #3: <0000000000000000>, tick_sched_timer, S:01, hrtimer_start, swapper/0/0
 # expires at 4247772000000-4247772000000 nsecs [in 291377847 to 291377847 nsecs]
 #4: def_rt_bandwidth, sched_rt_period_timer, S:01, start_bandwidth_timer, watchdog/1/27
 # expires at 4248000000000-4248000000000 nsecs [in 519377847 to 519377847 nsecs]
 #5: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, nautilus/2396
 # expires at 4248024543050-4248025501049 nsecs [in 543920897 to 544878896 nsecs]
 #6: <0000000000000000>, watchdog_timer_fn, S:01, hrtimer_start, watchdog/0/26
 # expires at 4248088000000-4248088000000 nsecs [in 607377847 to 607377847 nsecs]
 #7: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, wpa_supplicant/1124
 # expires at 4248212523760-4248213689856 nsecs [in 731901607 to 733067703 nsecs]
 #8: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, indicator-datet/2256
 # expires at 4248520033281-4248580019280 nsecs [in 1039411128 to 1099397127 nsecs]
 #9: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/1089
 # expires at 4251000599096-4251004595095 nsecs [in 3519976943 to 3523972942 nsecs]
 #10: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, rtkit-daemon/1664
 # expires at 4249533935864-4252033935864 nsecs [in 2053313711 to 4553313711 nsecs]
 #11: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, chrome/3694
 # expires at 4267018762427-4267018812427 nsecs [in 19538140274 to 19538190274 nsecs]
 #12: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, unity-settings-/2133
 # expires at 4269023758383-4269046874382 nsecs [in 21543136230 to 21566252229 nsecs]
 #13: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, Chrome_CacheThr/3324
 # expires at 4269666468701-4269696468700 nsecs [in 22185846548 to 22215846547 nsecs]
 #14: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, whoopsie/1156
 # expires at 7217000701195-7217100701195 nsecs [in 2969520079042 to 2969620079042 nsecs]
 #15: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, dhclient/1200
 # expires at 34125611678906-34125711678906 nsecs [in 29878131056753 to 29878231056753 nsecs]
 #16: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, evolution-sourc/2336
 # expires at 90105058846171-90105158846171 nsecs [in 85857578224018 to 85857678224018 nsecs]
 clock 1:
  .base:       0000000000000000
  .index:      1
  .resolution: 1 nsecs
  .get_time:   ktime_get_real
  .offset:     1411301051531545251 nsecs
active timers:
 #0: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, dropbox/2504
 # expires at 1411305312032309055-1411305312032359055 nsecs [in 1411301064551686902 to 1411301064551736902 nsecs]
 clock 2:
  .base:       0000000000000000
  .index:      2
  .resolution: 1 nsecs
  .get_time:   ktime_get_boottime
  .offset:     0 nsecs
active timers:
 clock 3:
  .base:       0000000000000000
  .index:      3
  .resolution: 1 nsecs
  .get_time:   ktime_get_clocktai
  .offset:     1411301051531545251 nsecs
active timers:
  .expires_next   : 4247488980657 nsecs
  .hres_active    : 1
  .nr_events      : 81595
  .nr_retries     : 2
  .nr_hangs       : 0
  .max_hang_time  : 0 nsecs
  .nohz_mode      : 2
  .last_tick      : 4247480000000 nsecs
  .tick_stopped   : 1
  .idle_jiffies   : 4295954166
  .idle_calls     : 372832
  .idle_sleeps    : 351322
  .idle_entrytime : 4247479199913 nsecs
  .idle_waketime  : 4247446306868 nsecs
  .idle_exittime  : 4247479115925 nsecs
  .idle_sleeptime : 4179306984887 nsecs
  .iowait_sleeptime: 11883637656 nsecs
  .last_jiffies   : 4295954166
  .next_jiffies   : 4295954240
  .idle_expires   : 4247772000000 nsecs
jiffies: 4295954167

cpu: 1
 clock 0:
  .base:       0000000000000000
  .index:      0
  .resolution: 1 nsecs
  .get_time:   ktime_get
  .offset:     0 nsecs
active timers:
 #0: <0000000000000000>, tick_sched_timer, S:01, hrtimer_start_range_ns, swapper/1/0
 # expires at 4247484000000-4247484000000 nsecs [in 3377847 to 3377847 nsecs]
 #1: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2505
 # expires at 4247489179637-4247489229637 nsecs [in 8557484 to 8607484 nsecs]
 #2: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, agnclientd/1284
 # expires at 4247497669912-4247497769911 nsecs [in 17047759 to 17147758 nsecs]
 #3: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, Xorg/1217
 # expires at 4247974309798-4247974805796 nsecs [in 493687645 to 494183643 nsecs]
 #4: <0000000000000000>, watchdog_timer_fn, S:01, hrtimer_start, watchdog/1/27
 # expires at 4248088000000-4248088000000 nsecs [in 607377847 to 607377847 nsecs]
 #5: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, unity-panel-ser/2143
 # expires at 4250023876877-4250027992876 nsecs [in 2543254724 to 2547370723 nsecs]
 #6: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2686
 # expires at 4250058467492-4250062463491 nsecs [in 2577845339 to 2581841338 nsecs]
 #7: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, upowerd/1631
 # expires at 4263000415782-4263029879781 nsecs [in 15519793629 to 15549257628 nsecs]
 #8: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, avahi-daemon/733
 # expires at 4304861977149-4304961977149 nsecs [in 57381354996 to 57481354996 nsecs]
 #9: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, unity-scope-loa/2931
 # expires at 5709058983921-5709158983921 nsecs [in 1461578361768 to 1461678361768 nsecs]
 clock 1:
  .base:       0000000000000000
  .index:      1
  .resolution: 1 nsecs
  .get_time:   ktime_get_real
  .offset:     1411301051531545251 nsecs
active timers:
 clock 2:
  .base:       0000000000000000
  .index:      2
  .resolution: 1 nsecs
  .get_time:   ktime_get_boottime
  .offset:     0 nsecs
active timers:
 clock 3:
  .base:       0000000000000000
  .index:      3
  .resolution: 1 nsecs
  .get_time:   ktime_get_clocktai
  .offset:     1411301051531545251 nsecs
active timers:
  .expires_next   : 4247484000000 nsecs
  .hres_active    : 1
  .nr_events      : 44796
  .nr_retries     : 3
  .nr_hangs       : 0
  .max_hang_time  : 0 nsecs
  .nohz_mode      : 2
  .last_tick      : 4247480000000 nsecs
  .tick_stopped   : 0
  .idle_jiffies   : 4295954166
  .idle_calls     : 271018
  .idle_sleeps    : 212182
  .idle_entrytime : 4247479271138 nsecs
  .idle_waketime  : 4247459294718 nsecs
  .idle_exittime  : 4247478828681 nsecs
  .idle_sleeptime : 4179358973317 nsecs
  .iowait_sleeptime: 27467059422 nsecs
  .last_jiffies   : 4295954166
  .next_jiffies   : 4295954167
  .idle_expires   : 4247864000000 nsecs
jiffies: 4295954167

cpu: 2
 clock 0:
  .base:       0000000000000000
  .index:      0
  .resolution: 1 nsecs
  .get_time:   ktime_get
  .offset:     0 nsecs
active timers:
 #0: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2946
 # expires at 4247489160788-4247489210788 nsecs [in 8538635 to 8588635 nsecs]
 #1: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, compiz/2241
 # expires at 4247655889685-4247656069684 nsecs [in 175267532 to 175447531 nsecs]
 #2: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, Watchdog/3336
 # expires at 4247809364046-4247809414046 nsecs [in 328741893 to 328791893 nsecs]
 #3: <0000000000000000>, tick_sched_timer, S:01, hrtimer_start, swapper/2/0
 # expires at 4247992000000-4247992000000 nsecs [in 511377847 to 511377847 nsecs]
 #4: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2610
 # expires at 4248024267044-4248028263043 nsecs [in 543644891 to 547640890 nsecs]
 #5: <0000000000000000>, watchdog_timer_fn, S:01, hrtimer_start, watchdog/2/32
 # expires at 4248100000000-4248100000000 nsecs [in 619377847 to 619377847 nsecs]
 #6: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2338
 # expires at 4250024247975-4250028243974 nsecs [in 2543625822 to 2547621821 nsecs]
 #7: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2964
 # expires at 4250058500538-4250062496537 nsecs [in 2577878385 to 2581874384 nsecs]
 #8: <0000000000000000>, hrtimer_wakeup, S:01, do_nanosleep, cron/1081
 # expires at 4250145366223-4250145416223 nsecs [in 2664744070 to 2664794070 nsecs]
 #9: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, agnLogd/1110
 # expires at 4251382905130-4251387905128 nsecs [in 3902282977 to 3907282975 nsecs]
 #10: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, agnclientd/1264
 # expires at 4251399250937-4251404250934 nsecs [in 3918628784 to 3923628781 nsecs]
 #11: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, NetworkManager/904
 # expires at 4253000967785-4253006963784 nsecs [in 5520345632 to 5526341631 nsecs]
 #12: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, chrome/3357
 # expires at 4254986820169-4254986870169 nsecs [in 7506198016 to 7506248016 nsecs]
 #13: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, kerneloops/1291
 # expires at 4302000021965-4303000021965 nsecs [in 54519399812 to 55519399812 nsecs]
 #14: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, update-notifier/2639
 # expires at 4304024139343-4304082256341 nsecs [in 56543517190 to 56601634188 nsecs]
 #15: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gconfd-2/2422
 # expires at 4304058518496-4304118483494 nsecs [in 56577896343 to 56637861341 nsecs]
 #16: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, udisksd/2426
 # expires at 4305000286804-4305100286804 nsecs [in 57519664651 to 57619664651 nsecs]
 #17: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, zeitgeist-daemo/2590
 # expires at 4324058584556-4324158584556 nsecs [in 76577962403 to 76677962403 nsecs]
 #18: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, unity-scope-hom/2918
 # expires at 4509058373979-4509158373979 nsecs [in 261577751826 to 261677751826 nsecs]
 #19: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, rsyslogd/417
 # expires at 86412167178201-86412267178201 nsecs [in 82164686556048 to 82164786556048 nsecs]
 #20: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, indicator-print/2263
 # expires at 86443024594399-86443124594399 nsecs [in 82195543972246 to 82195643972246 nsecs]
 #21: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, cupsd/2017
 # expires at 86504291962236-86504391962236 nsecs [in 82256811340083 to 82256911340083 nsecs]
 clock 1:
  .base:       0000000000000000
  .index:      1
  .resolution: 1 nsecs
  .get_time:   ktime_get_real
  .offset:     1411301051531545251 nsecs
active timers:
 #0: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, dropbox/2482
 # expires at 1411305371788086891-1411305371788136891 nsecs [in 1411301124307464738 to 1411301124307514738 nsecs]
 #1: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, dropbox/2484
 # expires at 1411305874964544057-1411305874964594057 nsecs [in 1411301627483921904 to 1411301627483971904 nsecs]
 clock 2:
  .base:       0000000000000000
  .index:      2
  .resolution: 1 nsecs
  .get_time:   ktime_get_boottime
  .offset:     0 nsecs
active timers:
 clock 3:
  .base:       0000000000000000
  .index:      3
  .resolution: 1 nsecs
  .get_time:   ktime_get_clocktai
  .offset:     1411301051531545251 nsecs
active timers:
  .expires_next   : 4247489210788 nsecs
  .hres_active    : 1
  .nr_events      : 95808
  .nr_retries     : 0
  .nr_hangs       : 0
  .max_hang_time  : 0 nsecs
  .nohz_mode      : 2
  .last_tick      : 4247480000000 nsecs
  .tick_stopped   : 1
  .idle_jiffies   : 4295954166
  .idle_calls     : 284009
  .idle_sleeps    : 263125
  .idle_entrytime : 4247479794416 nsecs
  .idle_waketime  : 4247475913774 nsecs
  .idle_exittime  : 4247479779863 nsecs
  .idle_sleeptime : 4190282408893 nsecs
  .iowait_sleeptime: 11837597253 nsecs
  .last_jiffies   : 4295954166
  .next_jiffies   : 4295954295
  .idle_expires   : 4247992000000 nsecs
jiffies: 4295954167

cpu: 3
 clock 0:
  .base:       0000000000000000
  .index:      0
  .resolution: 1 nsecs
  .get_time:   ktime_get
  .offset:     0 nsecs
active timers:
 #0: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2488
 # expires at 4247489285641-4247489335641 nsecs [in 8663488 to 8713488 nsecs]
 #1: <0000000000000000>, hrtimer_wakeup, S:01, do_nanosleep, syndaemon/2337
 # expires at 4247499895080-4247499945080 nsecs [in 19272927 to 19322927 nsecs]
 #2: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, Chrome_IOThread/3325
 # expires at 4247541559355-4247541659354 nsecs [in 60937202 to 61037201 nsecs]
 #3: <0000000000000000>, tick_sched_timer, S:01, hrtimer_start, swapper/3/0
 # expires at 4247560000000-4247560000000 nsecs [in 79377847 to 79377847 nsecs]
 #4: <0000000000000000>, hrtimer_wakeup, S:01, do_nanosleep, nautilus/2502
 # expires at 4248066463764-4248066513764 nsecs [in 585841611 to 585891611 nsecs]
 #5: <0000000000000000>, watchdog_timer_fn, S:01, hrtimer_start, watchdog/3/37
 # expires at 4248116000000-4248116000000 nsecs [in 635377847 to 635377847 nsecs]
 #6: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, gmain/2970
 # expires at 4250058098880-4250062093879 nsecs [in 2577476727 to 2581471726 nsecs]
 #7: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, Chrome_SyncThre/3355
 # expires at 4251399823644-4251399873644 nsecs [in 3919201491 to 3919251491 nsecs]
 #8: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, chrome/3617
 # expires at 4252963118137-4252963168137 nsecs [in 5482495984 to 5482545984 nsecs]
 #9: <0000000000000000>, hrtimer_wakeup, S:01, do_nanosleep, irqbalance/1007
 # expires at 4253468456971-4253468506971 nsecs [in 5987834818 to 5987884818 nsecs]
 #10: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, rtkit-daemon/1665
 # expires at 4254533976077-4254533976077 nsecs [in 7053353924 to 7053353924 nsecs]
 #11: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, Chrome_HistoryT/3346
 # expires at 4266594279111-4266594329111 nsecs [in 19113656958 to 19113706958 nsecs]
 #12: <0000000000000000>, hrtimer_wakeup, S:01, hrtimer_start_range_ns, WorkerPool/3706/3706
 # expires at 4524724506291-4524724556291 nsecs [in 277243884138 to 277243934138 nsecs]
 #13: <0000000000000000>, hrtimer_wakeup, S:01, schedule_hrtimeout_range_clock, Chrome_FileThre/3321
 # expires at 5075335901973-5075435901973 nsecs [in 827855279820 to 827955279820 nsecs]
 clock 1:
  .base:       0000000000000000
  .index:      1
  .resolution: 1 nsecs
  .get_time:   ktime_get_real
  .offset:     1411301051531545251 nsecs
active timers:
 clock 2:
  .base:       0000000000000000
  .index:      2
  .resolution: 1 nsecs
  .get_time:   ktime_get_boottime
  .offset:     0 nsecs
active timers:
 clock 3:
  .base:       0000000000000000
  .index:      3
  .resolution: 1 nsecs
  .get_time:   ktime_get_clocktai
  .offset:     1411301051531545251 nsecs
active timers:
  .expires_next   : 4247489335641 nsecs
  .hres_active    : 1
  .nr_events      : 56209
  .nr_retries     : 3
  .nr_hangs       : 0
  .max_hang_time  : 0 nsecs
  .nohz_mode      : 2
  .last_tick      : 4247480000000 nsecs
  .tick_stopped   : 1
  .idle_jiffies   : 4295954166
  .idle_calls     : 209673
  .idle_sleeps    : 186763
  .idle_entrytime : 4247479299032 nsecs
  .idle_waketime  : 4247474694194 nsecs
  .idle_exittime  : 4247478692742 nsecs
  .idle_sleeptime : 4179567182012 nsecs
  .iowait_sleeptime: 21767231547 nsecs
  .last_jiffies   : 4295954166
  .next_jiffies   : 4295954187
  .idle_expires   : 4247560000000 nsecs
jiffies: 4295954167

Tick Device: mode:     1
Broadcast device
Clock Event Device: hpet
 max_delta_ns:   149983013276
 min_delta_ns:   13410
 mult:           61496111
 shift:          32
 mode:           1
 next_event:     9223372036854775807 nsecs
 set_next_event: hpet_legacy_next_event
 set_mode:       hpet_legacy_set_mode
 event_handler:  tick_handle_oneshot_broadcast
 retries:        0

tick_broadcast_mask: 00000000
tick_broadcast_oneshot_mask: 00000000

Tick Device: mode:     1
Per CPU device: 0
Clock Event Device: lapic
 max_delta_ns:   1632563005815
 min_delta_ns:   1000
 mult:           11299254
 shift:          27
 mode:           3
 next_event:     4247488980657 nsecs
 set_next_event: lapic_next_deadline
 set_mode:       lapic_timer_setup
 event_handler:  hrtimer_interrupt
 retries:        0

Tick Device: mode:     1
Per CPU device: 1
Clock Event Device: lapic
 max_delta_ns:   1632563005815
 min_delta_ns:   1000
 mult:           11299254
 shift:          27
 mode:           3
 next_event:     4247484000000 nsecs
 set_next_event: lapic_next_deadline
 set_mode:       lapic_timer_setup
 event_handler:  hrtimer_interrupt
 retries:        0

Tick Device: mode:     1
Per CPU device: 2
Clock Event Device: lapic
 max_delta_ns:   1632563005815
 min_delta_ns:   1000
 mult:           11299254
 shift:          27
 mode:           3
 next_event:     4247489210788 nsecs
 set_next_event: lapic_next_deadline
 set_mode:       lapic_timer_setup
 event_handler:  hrtimer_interrupt
 retries:        0

Tick Device: mode:     1
Per CPU device: 3
Clock Event Device: lapic
 max_delta_ns:   1632563005815
 min_delta_ns:   1000
 mult:           11299254
 shift:          27
 mode:           3
 next_event:     4247489335641 nsecs
 set_next_event: lapic_next_deadline
 set_mode:       lapic_timer_setup
 event_handler:  hrtimer_interrupt
 retries:        0
```

## Scheduler

xv6 uses round-robin scheduler. It does not support preemption. Every process
keeps running until calling exit system call or becoming sleeping. For SMP
machie, PIT is used at first. The APIC local timer replaces it. Only CPU 0
updates ticks when receiving a timer interrupt.

jos also uses a round-robin schedulr. But it supports preemption. It does
preemption for IRQ_TIMER, IRQ_KBB and IRQ_SERIAL interrupts.

Linux uses Completely Fair Schedulere (CFS). Two key parameters are target
latency and minimum granularity.

Both runqueue and waitqueue are per-cpu structures. runqueue is red-black tree.
waitqueue is linked list.

Scheduler operation

- Add a task to runqueue
- Remove a task from runqueue
- Pick next task from runqueue


## References
- [xv6](http://pdos.csail.mit.edu/6.828/2014/xv6.html)
- [jos](http://pdos.csail.mit.edu/6.828/2014/labguide.html)
- `Linux Kernel Development`
- `Understanding the Linux Kernel`
- time(7) man page
- [Kernel Timer Systems](http://elinux.org/Kernel_Timer_Systems)
- Linux/Documentation/timers
- [Why the count of IO-APIC-edge timer showed in /proc/interrupts does not change often?](http://stackoverflow.com/questions/25931134/why-the-count-of-io-apic-edge-timer-showed-in-proc-interrupts-does-not-change-o)

- [CHAPTER 15. TIMESTAMPING](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_for_real_time/7/html/reference_guide/chap-timestamping)
- [Switching clock from TSC to HPET on Linux](https://gist.github.com/tobert/8063382)
- [It's Go Time on Linux](https://blog.cloudflare.com/its-go-time-on-linux/)
-  [Timers and time management in the Linux kernel. Part 6.](https://0xax.gitbooks.io/linux-insides/Timers/linux-timers-6.html)

- [Linux 时钟管理](http://www.ibm.com/developerworks/cn/linux/l-cn-timerm/)
- [Ingo Molnar's Response on lkml](https://lkml.org/lkml/2005/10/19/46)
- [(Nearly) full tickless operation in 3.10](http://lwn.net/Articles/549580/)
