# Update version and test Operator Locally with OLM on a Kind cluster

## Prerequisite:

## Action Steps:
   1. Go to your clone Integrity Shield repository
      
      ```
      cd <Home Dir>/github.com/IBM/integrity-enforcer
      ```
   2. Set env variable
   
      ```
      export ISHIELD_ENV=local
      export ISHIELD_REPO_ROOT=<Home Dir>/github.com/IBM/integrity-enforcer
      export TEST_LOCAL=true
       export KUBECONFIG=<Home Dir>/github.com/IBM/integrity-enforcer/integrity-shield-operator/kubeconfig_managed
      ```
   3. Change version in build configuration
      
      ```
      nano ishield-build.conf
      ```
      Edit the following properties as shown below.
      
      ![Shield Config](./shield-config.PNG)
      
      
      Before
        ISHIELD_VERSION=0.1.5
        VERSION=0.1.5
        PREV_VERSION=0.1.4
        
       After     
        ISHIELD_VERSION=0.1.6
        VERSION=0.1.6
        PREV_VERSION=0.1.5
      
      
   5. Reset kind cluster 
       
      ```
        make delete-private-registry
        make delete-kind-cluster
        make create-kind-cluster
     
      ```
        
   7. Make sure all pods in kind cluster in ready state. (wait for serveral (5) minutes)
      
      ```
      kubectl get pod --all-namespaces
      ```
      
   8. Make sure env variable KUBECONFIG points to Kind cluster
     
      ```
      export KUBECONFIG=<Home Dir>/github.com/IBM/integrity-enforcer/integrity-shield-operator/kubeconfig_managed
      ```
   9. Run e2e test using OLM bundle and make sure all e2e tests are passed
      
      ```
      make test-e2e-bundle
      ```
   10. Update version in required files
      
      ```
       make update-version
      ```
       
   12. Check git status and git diff for all changed files

   13. Perform manual edit on `<Home Dir>/github.com/IBM/integrity-enforcer/integrity-shield-operator/bundle/manifests/integrity-shield-operator.clusterserviceversion.yaml`

       - Add line `replaces: integrity-shield-operator.vx.x.x`   (x matches with previous PREV_VERSION in `ishield-build.conf`)
       - Change from localhost:5000 to quay.io/open-cluster-management in all places.
       - Save the file
       
   14. Commit changes to files and create PR
