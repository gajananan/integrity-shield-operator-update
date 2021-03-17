# Test Integrity Shield with RHACM

## Prerequisite:

## Action Steps:

 1. Git clone your forked repository for : https://github.com/open-cluster-management/policy-collection
 
 2. Edit the policy file community/CM-Configuration-Management/policy-integrity-shield.yaml
 
 3. Add the following ConfigurationPolicy to policy-integrity-shield.yaml,   under policy-templates:
    
    - change version in image: (quay.io/open-cluster-management/integrity-shield-operator-index:0.1.5)
    
      ```YAML
        - objectDefinition:
        apiVersion: policy.open-cluster-management.io/v1
        kind: ConfigurationPolicy
        metadata:
          name: policy-integrity-shield-catrsc
        spec:
          remediationAction: enforce
          severity: high
          object-templates:
            - complianceType: musthave
              objectDefinition:
                apiVersion: operators.coreos.com/v1alpha1
                kind: CatalogSource
                metadata:
                  name: integrity-shield-operator-catalog
                  namespace: openshift-marketplace
                spec:
                  displayName: Integrity Shield Operator
                  image: quay.io/open-cluster-management/integrity-shield-operator-index:0.1.5
                  publisher: IBM
                  sourceType: grpc
                  updateStrategy:
                    registryPoll:
                      interval: 15m
      ```      
    
    - Change the following ConfigurationPolicy in policy-integrity-shield.yaml, 
    
      Update the following fields
          source: integrity-shield-operator-catalog   <should be same as the CatalogSource name used in 2.1>
          startingCSV: integrity-shield-operator.v0.1.5  <update version based on latest version>
              
      The following is an example after the changes.
      
      ![policy-changes](./policy-changes.png)
      
 4. Commit the changes to policy and test policy as described in [doc] (https://github.ibm.com/integrity-shield/integrity-shield-testscenarios/blob/fix/install-scenarios/TEST_QUICK_README.md   )
