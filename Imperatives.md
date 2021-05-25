# Imperative commands

# General

> `alias k=kubectl`

## Namespace

> `k get ns` - Lists all the namespaces

> `k config get-contexts` - Can be used to check the current working namespace

> `k config set-context --current --namespace <namespace-name>` - Used to set the current working namespace

> `k -n <namespace> [REST OF THE COMMAND]` - Executes the command in the given namespace  
`k -n my-namespace get all -o wide`

> `k create ns <new-namespace>` - Creates a new namespace  
`k create ns monitoring` - Creates a namespace called _monitoring_