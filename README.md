# resctl-demo-image-recipe
The recipes will build the Debian-based image for the resctl-demo.


# Requirements
Debian system with debos and qemu installed.


# Build ospack

Place binary packages for `resctl-demo` and `resctl-demo-linux` under the `debs/` directory.
See the `.gitlab-ci.yml` for further instructions.

    $ mkdir out && cd out
    $ debos --scratchsize=16G ../resctl-demo-ospack.yaml


# Build image & run under QEmu for local testing

    $ cd out
    $ debos --scratchsize=16G -t imagesize:60GB ../resctl-demo-image.yaml
    $ ../start-qemu.sh

To test the root pivot service, use `start-qemu-pivot.sh` to add a second disk.


# Build image & upload to AWS EC2

Some environment variables need to be set to your EC2 secrets:

    EC2_ACCESS_ID, EC2_SECRET_KEY, EC2_REGION, EC2_BUCKET


    $ cd out
    $ debos --scratchsize=16G ../resctl-demo-image.yaml
    $ python3 ../upload-image-aws-ec2.py --ami-name="resctl-demo" --ami-description="resctl-demo" --image-file="resctl-demo-image.vmdk"
