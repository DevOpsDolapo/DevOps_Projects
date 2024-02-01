
## Implementing Ansible Dynamic Assignments (Include) and Community Roles

In this project, we will introduce `dynamic assignments` by using the `include` module. The major difference between static and dynamic assignments is that static assignments use the `import` Ansible module, while dynamic assignments use `include` module.

When the `import` module is used, all statements are pre-processed at the time playbooks are parsed, which means that Ansible will process all the playbooks referenced during the time it is parsing the statements. This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when the `include` module is used, all statements are processed only during execution of the playbook. Which means that after the statements are parsed, any changes to the statements encountered during execution will be used.

Generally, the recommendation is to use static assignments for playbooks, because it is more reliable. Debugging playbook problems in dynamic assignments can be difficult due to its dynamic nature. However, dynamic assignments can be used for environment specific variables.

### Introducing Dynamic Assignment into The Current Structure

**Step 1: Create a new branch `dynamic-assignments` in the `https://github.com/DevOpsDolapo/ansible-config-mgt.git` GitHub repository**

- Create a new folder and name it `dynamic-assignments`

