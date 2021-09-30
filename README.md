# terraform-random-provider

# Why it was created
When working with multiple resources configurations it is easier a tool to create randomness.

# Theory
Random values would be created and used within a resource on the creation time.\
Resources would hold that generated random value unchanged till the input for the resource would change.

# Random types:
- id
- integer
- password
- pet
- shuffle
- string
- uuid

# Example
Bellow block code shows an example using random_pet resource.
```
resource "random_pet" "my-pet" {
  prefix = "Mr"
  separator = "."
  length = "1"
}
```
Above resource will always hold the same data unless the arguments will change (for example the prefix or length).
Terraform will notice the change by applying a diff.


# Sample output
```
terraform apply -auto-approve

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_pet.my-pet will be created
  + resource "random_pet" "my-pet" {
      + id        = (known after apply)
      + length    = 1
      + prefix    = "Mr"
      + separator = "."
    }

Plan: 1 to add, 0 to change, 0 to destroy.
random_pet.my-pet: Creating...
random_pet.my-pet: Creation complete after 0s [id=Mr.mudfish]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
[ion:~/Documents/ … m-proj/output/tmp3] $ 
```

# Sample putput on resource change
_argument prefix was changed from "Mr" to "Fluffy"
```
terraform apply -auto-approve
random_pet.my-pet: Refreshing state... [id=Mr.mudfish]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
-/+ destroy and then create replacement

Terraform will perform the following actions:

  # random_pet.my-pet must be replaced
-/+ resource "random_pet" "my-pet" {
      ~ id        = "Mr.mudfish" -> (known after apply)
      ~ prefix    = "Mr" -> "Fluffy" # forces replacement
        # (2 unchanged attributes hidden)
    }

Plan: 1 to add, 0 to change, 1 to destroy.
random_pet.my-pet: Destroying... [id=Mr.mudfish]
random_pet.my-pet: Destruction complete after 0s
random_pet.my-pet: Creating...
random_pet.my-pet: Creation complete after 0s [id=Fluffy.polliwog]

Apply complete! Resources: 1 added, 0 changed, 1 destroyed.
[ion:~/Documents/ … m-proj/output/tmp3] $ 
```


