# docker-compose-on-aws-ecs

## WARNING: work in progress

## Contents

1. [Steps](#steps)
    1. [AWS metadata](#aws-metadata)
    1. [Identify project](#identify-project)
    1. [Create launch configuration](#create-launch-configuration)
    1. [Create auto-scaling-group-provider](#create-auto-scaling-group-provider)
    1. [Create capacity provider](#create-capacity-provider)
1. [Cleanup](#cleanup)

## Steps

### AWS metadata

1. :pencil2: AWS metadata used by multiple CLI invocations.
   Example:

    ```console
    export AWS_REGION=us-east-1
    export AWS_AVAILABILITY_ZONE=us-east-1e
    export AWS_KEYPAIR=aws-default-key-pair
    ```

### Identify project

1. :pencil2: XXX.
   Example:

    ```console
    export AWS_PROJECT=project01
    ```

### Create launch configuration

1. References:
    1. [AWS CLI Command reference](https://docs.aws.amazon.com/cli/latest/index.html)
        1. [aws](https://docs.aws.amazon.com/cli/latest/reference/index.html#cli-aws)
           [autoscaling](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/index.html#cli-aws-autoscaling)
           [create-launch-configuration](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-launch-configuration.html)

1. Create launch configuration.
   Example:

    ```console
    aws autoscaling create-launch-configuration \
      --launch-configuration-name ${AWS_PROJECT}-launch-configuration-name \
      --image-id ami-09d95fab7fff3776c \
      --instance-type t2.micro \
      --key-name ${AWS_KEYPAIR}
    ```

1. Verify in AWS Console: [Launch Configurations](https://console.aws.amazon.com/ec2/autoscaling/home)

### Create auto-scaling-group-provider

1. References:
    1. [AWS CLI Command reference](https://docs.aws.amazon.com/cli/latest/index.html)
        1. [aws](https://docs.aws.amazon.com/cli/latest/reference/index.html#cli-aws)
           [autoscaling](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/index.html#cli-aws-autoscaling)
           [create-auto-scaling-group](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-auto-scaling-group.html)

1. Create autoscaling group.
   Example:

    ```console
    aws autoscaling create-auto-scaling-group \
      --auto-scaling-group-name ${AWS_PROJECT}-auto-scaling-group-name \
      --availability-zones ${AWS_AVAILABILITY_ZONE} \
      --launch-configuration-name ${AWS_PROJECT}-launch-configuration-name \
      --max-size 2 \
      --min-size 1
    ```

1. Verify in AWS Console: [Autoscaling](https://console.aws.amazon.com/ec2/autoscaling/home)

### Create capacity provider

FIXME: Doesn't work.

1. References:
    1. [AWS CLI Command reference](https://docs.aws.amazon.com/cli/latest/index.html)
        1. [aws](https://docs.aws.amazon.com/cli/latest/reference/index.html#cli-aws)
           [ecs](https://docs.aws.amazon.com/cli/latest/reference/ecs/index.html#cli-aws-ecs)
           [create-capacity-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-capacity-provider.html)

1. XXX.
   Example:

    ```console
    aws ecs create-capacity-provider \
      --name  ${AWS_PROJECT}-capacity-provider \
      --auto-scaling-group-provider "AutoScalingGroupArn=arn:aws:iam::488776654093:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling,managedScaling={status=DISABLED,targetCapacity=1,minimumScalingStepSize=1,maximumScalingStepSize=1},managedTerminationProtection=DISABLED"
    ```

1. Verify in AWS Console: [????]()

## Cleanup

1. Cleanup.
   Example:

    ```console
    aws autoscaling delete-auto-scaling-group \
      --auto-scaling-group-name ${AWS_PROJECT}-auto-scaling-group-name \
      --force-delete

    aws autoscaling delete-launch-configuration \
      --launch-configuration-name ${AWS_PROJECT}-launch-configuration-name
    ```

1. Verify in AWS Console
    1. [Autoscaling](https://console.aws.amazon.com/ec2/autoscaling/home)
    1. [Launch Configurations](https://console.aws.amazon.com/ec2/autoscaling/home)