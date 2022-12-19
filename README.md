# Hosted zone level restriction in route 53 using custom IAM policy
=============================================================================
 By default, you can't give access to to a IAM user to a specific zones.instead you have to create your own custom IAM policy to perform actions on a specific hosted zone in route 53.
 
 Simply route 53 is the managed DNS service provided by AWS which allows you to purchase domains and manage your domains by creating zones for the domains.
 
 In this example the IAM user will be able to view the zones in the account but he won't be able to view /perform actions other than the permitted zone.
 
 You can refer to the following images to get a clear idea about this.
 
 
 
 before proceeding further, you should create a IAM user from the console and you don't need to attach any permissions to the user.
 
 So let's start.
 
 
- ###### login to your AWS console.
- ###### Create a IAM user with no privilges,(You can add other privileges if you need, in this example we are only allowing the user to view the zones and make changes to only his hosted zone)
- ###### Add  atgs to the user if required.
- ###### Once you created the user select the user and under the permissions option click on add inline polcy.
- ###### Choose Json from the list and add the following code.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:ListHostedZonesByName",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZones"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                            "route53:*"
            ],
            "Resource": "arn:aws:route53:::hostedzone/Z00807807EZOKH2E86UP<hosted zoneID>"
        }
    ]
}
```
-  ###### Let's review the above code. The IAM user was granted to perform the following operations on route 53
-  ListHostedZonesByName
-  GetHostedZoneCount,
-  ListHostedZones 
  which is required to list all the zone files inside the account and it was defined in the first statement.

In the second statement the IAM user is granted to perform all the operations on the specified hosted zone and it is denoted by  "*".

You have to replace the hostedzone ID with your's.

You can find the hosted zoneID by expanding the hosted zone details.

In the above example it was Z00807807EZOKH2E86UP, you can replace the value with yours.

Congratulations, you have configured the IAM policy successfully.
