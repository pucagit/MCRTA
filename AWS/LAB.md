# AWS Cloud Red Teaming

## 1. Full URL of the s3 bucket belongs to “cwl-metatech” organization?

```sh
$ ./cloud_enum.py -k cwl-metatech --disable-azure --disable-gcp

##########################
        cloud_enum
   github.com/initstring
##########################


Keywords:    cwl-metatech
Mutations:   /home/kali/cloud_enum/enum_tools/fuzz.txt
Brute-list:  /home/kali/cloud_enum/enum_tools/fuzz.txt

[+] Mutations list imported: 306 items
[+] Mutated results: 1837 items

++++++++++++++++++++++++++
      amazon checks
++++++++++++++++++++++++++

[+] Checking for S3 buckets
<SNIP>
  OPEN S3 BUCKET: http://cwl-metatech-prod.s3.amazonaws.com/
      FILES:
      ->http://cwl-metatech-prod.s3.amazonaws.com/cwl-metatech-prod
      ->http://cwl-metatech-prod.s3.amazonaws.com/dev-server-ip.txt
      ->http://cwl-metatech-prod.s3.amazonaws.com/prod-data.txt
      ->http://cwl-metatech-prod.s3.amazonaws.com/staging-data.txt
<SNIP>
```

## 2. Web app running on “dev-server” ec2 instance, what is name of the parameter which is vulnerable to SSRF?

- Visit http://cwl-metatech-prod.s3.amazonaws.com/dev-server-ip.txt -> obtain the “dev-server” ec2 instance IP at 18.222.161.152
- 

## 3. Name of role attached to the dev ec2 instance?
## 4. Name of the user who is part of the “interns” group.
## 5. Name of the group with an “emp003” user as a member.
## 6. Account ID of the user which can assume “crossaccount-role”.
## 7. Name of aws role which can be assumed by “devops-role”?
## 8. Name of Inline policy embed to “emp001” user.
## 9. ARN of policy attached to “employees” group?
## 10. The credit card number of “Bob” is in the “prod-data.txt” file stored in the s3 bucket.