Anthos CI/CD cluster variants
==================================================

# NAME

  cluster-variants

# SYNOPSIS

To fetch the package with variants:
    
    # pulls the cluster-variants package and all sub-packages along with their dependencies
    $ kpt pkg get git@github.com:phanimarupaka/clusters-variants.git@v0.1.0 variants/
    
    fetching package git@github.com:phanimarupaka/clusters-variants.git@v0.1.0 to variants/
    fetching dependency package git@github.com:phanimarupaka/clusters-base.git@v0.1.0 to variants/dev/base/
    fetching dependency package git@github.com:phanimarupaka/clusters-base.git@v0.1.0 to variants/staging/base/

The fetched package would look like this: 

https://github.com/phanimarupaka/fetched-variants/tree/master/clusters-variants

To list setters in the base:
    
    cd variants;
    
    kpt cfg list-setters ./dev/base
    
         NAME           VALUE        SET BY            DESCRIPTION             COUNT   REQUIRED  
      environment   ENV                       Environment of the cluster       21      Yes       
      namespace     YOUR_NAMESPACE            Namespace of Config Connector    3       Yes       
                                              resources                                          
      project-id    YOUR_PROJECT              Project ID where the cluster     1       Yes       
                                              will run                                           
      region        REGION                    Region of the cluster            21      Yes       
      
    
To set and apply the dev (cli experience):

    $ kpt cfg set dev/base environment dev
    set 21 values
    
    $ kpt cfg set dev/base namespace dev-namespace
    set 3 values
    
    $ kpt cfg set dev/base  project-id my-dev-project
    set 3 values
   
    $ kpt cfg set dev/base region us-central-1
    set 21 values
    
    $ kubectl apply -r -f dev/
    
To set and apply the dev (declarative experience):

    # Pass the setter values to Kptfile in dev variant
    apiVersion: kpt.dev/v1alpha1
    kind: Kptfile
    metadata:
        name: cluster-dev
    packageMetadata:
        shortDescription: sample description
    dependencies:
      - name: base
        git:
            repo: git@github.com:phanimarupaka/clusters-base
            directory: /
            ref: v0.1.0
        updateStrategy: resource-merge
        setters:
          environment: dev
          namespace: dev-space
          project-id: dev-project
          region: us-central-1
          
    # hydrate the dev package
    $ kpt pkg hydrate dev/
    set 21 values for setter environment with value dev
    set 3 values for setter namespace with value dev-namespace
    set 3 values for setter project-id with value my-dev-project
    set 21 values for setter region with value us-central-1
    
    $ kubectl apply -r -f dev/

# Description

You can apply this across multiple environments and regions by setting appropriate setters

