# Dask Sprint - Computational


1. Setting up k8s is pretty hard - major stumbling block
    2. Lots of tech assumes k8s, and that makes a big stumbling block
    3. Much less costly from TCO to use large machines than clusters. Much easier from a damage control perspective
    4. Clusters suck
    5. jrun eqivalents are being used to run interactive sessions
    6. Stats cluster is not really used
    7. Cost in learning slurm is pretty high. SLurm in our workflow is you need to refactor your code on how you work. Might be using things that don't work on clustered setups. 
    8. Debugging & access to resources is harder. If you can use the same code, it gets easier. 'Access to resources' -> storage scaling at a cluster level is expensive. Easier to get storage on a workstation, much less on a cluster. Hard to make infrastructure that makes everyone see the same storage - puts the problem on the user
    9. Cluster admins are super conservative. Old operating systems. Commandline launching utilities. Docker is not accessible. Silo'd politics of sysadmins. 
    10. Restrictive memory usage is bad, but can be higher up. 
    11. One computer you can oversubscribe. But at large scales you can not. Scalability of users. 
    12. Billing / incentive to be parsimoniuous. Groups should be able to see group dynamics & self police - see how much people are using what. Nested groups? See resource usage in the groups you are part of. A high-score table.
    13. Move control over resources + environment closer to users.
    14. Elastic computing is important - mostly you don't want to use all resources.
    15. HIerarchy of needs - you need to be able to experiment yourself first, and then go off to work with someone who knows what's doing. 
    16. Sysadmins are understaffed & grumpy. They are very behind times in many places. Need more investment in sysadmins.
    17. Mutualizing resources & billing them?
    18. Berkeley Research computing is very useful for this case.
    19. PI want to invest money & get a ROI. Everyone has diffe4rent needs. (I) can provide value by making dynamic environments / profiles possible. Something like kubespawner profile.
    20. Users get assigned whole nodes, of different sizes. You choose what you wanna get. The learning process is so high. Bastions and shit. This shit is fucked up. 
    21. Teams that should not be using 100s of TB of disks are because they could. 
    22. What to show users? How to guide them?
    23. Hopeful that JupyterLab will help. Dashboarding - JupyterHub / JupyterLab. How to easily understand
    24. Pangeo pain points - by default you don't have root access. Easier root access. But problematic for.
    25. Environment customizations is a problem. Need fixes.
    26. Interacting with data that is far away directly without having to copy it locally is a big deal. Not common in other languages / communities. Is hard. There is lots of work. 
    27. Nomachine alternative is going to be pretty good. Lots of people use it.
    28. Old code that takes forever to run in specific environments (nelle talks about complex pipelines)
    29. lolmatlab
    30. make generates basah script that runs on cluster?! (see Nelle). Build pipelines that do all the work and you can run simply. Hard to compose through. Talking to different languages is harder.
    31. Process paradigm (piping) vs shared memory (matthias' notebook) is useful.
    32. Help with archaic clusters. Boilerplate email to send to admins to convince them. Outreach is hard, make it easier.
    33. HPC is hard. I can't use your stuff (Gael). Tons of social problems. Solve my problems. Once you get a large enough tool, misuse and abuse becomes a problem. From a sysadmin pov everything is unsafe. This is the conflict that is fundamental to this - Security vs Usability.
    34. Singularity is a social success. 
    35. 10y ago, people would be like 'learn C++, forget python and come back'. Come back to do.
    36. Dashboards in the spawn environment is important. Provide in-place feedback when selecting spawn.
    37. Don't push too much complexity to sysadmins from users. Balance that between users, sysadmins & tool creators.
    38. Do lots of documentation & training.
    39. You can have one big machine, but can't have tons of big machines. Since you need puppet & what not.
    40. Some things are better solved with a couple of big boxes than a cluster.
    41. Don't want to change how students write code. You won't need it if you don't have too large data. Many people don't have large data, but maybe they don't because it is hard to process?
    42. 