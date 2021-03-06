

[::Go back to Oozie Documentation Index::](index.html)

# Workflow ReRun

<!-- MACRO{toc|fromDepth=1|toDepth=4} -->
## Configs

   * oozie.wf.application.path
   * Only one of following two configurations is mandatory. Both should not be defined at the same time.
      * `oozie.wf.rerun.skip.nodes`
      * `oozie.wf.rerun.failnodes`
   * Skip nodes are comma separated list of action names. They can be any action nodes including decision node.
   If this value is empty, or null, and `oozie.wf.rerun.failnodes` is false, than the whole workflow is rerunned.
   * The valid value of  `oozie.wf.rerun.failnodes` is true or false. By default its value is false. In case of setting
   this value to true, all the nodes where the status is OK will be skipped.
   * If secured hadoop version is used, the following two properties needs to be specified as well
      * mapreduce.jobtracker.kerberos.principal
      * dfs.namenode.kerberos.principal.
   * Configurations can be passed as -D param.

```
$ oozie job -oozie http://localhost:11000/oozie -rerun 14-20090525161321-oozie-joe -Doozie.wf.rerun.skip.nodes=<>
```

## Pre-Conditions

   * Workflow with id wfId should exist.
   * Workflow with id wfId should be in SUCCEEDED/KILLED/FAILED.
   * If specified , nodes in the config `oozie.wf.rerun.skip.nodes` must be completed successfully.

## ReRun

   * Reloads the configs.
   * If no configuration is passed, existing coordinator/workflow configuration will be used. If configuration is passed then, it will be merged with existing workflow configuration. Input configuration will take the precedence.
   * Currently there is no way to remove an existing configuration but only override by passing a different value in the input configuration.
   * Creates a new Workflow Instance with the same wfId.
   * Deletes the actions that are not skipped from the DB and copies data from old Workflow Instance to new one for skipped actions.
   * Action handler will skip the nodes given in the config with the same exit transition as before.

[::Go back to Oozie Documentation Index::](index.html)


