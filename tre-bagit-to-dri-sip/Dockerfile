FROM public.ecr.aws/lambda/python:3.8

# Loading Variables from Github Actions environment
ARG AWS_CODEARTIFACT_REPOSITORY_NAME
ARG AWS_CODEARTIFACT_REPOSITORY_DOMAIN
ARG AWS_CODEARTIFACT_REPOSITORY_ACCOUNT
ARG AWS_REGION

# Copy function code
COPY tre_bagit_to_dri_sip.py ${LAMBDA_TASK_ROOT}
COPY tre_bagit.py ${LAMBDA_TASK_ROOT}
COPY tre_bagit_transforms.py ${LAMBDA_TASK_ROOT}
# Copy schemas that will be added to dri-sip
COPY closure-schema.txt ${LAMBDA_TASK_ROOT}
COPY metadata-schema.txt ${LAMBDA_TASK_ROOT}

# Install the function's dependencies using file requirements.txt
COPY "*.whl" ./
COPY tmp-build-requirements.txt ./requirements.txt
RUN  yum -y update \
    && yum clean all \
    && aws codeartifact login --tool pip --repository ${AWS_CODEARTIFACT_REPOSITORY_NAME} --domain ${AWS_CODEARTIFACT_REPOSITORY_DOMAIN} --domain-owner ${AWS_CODEARTIFACT_REPOSITORY_ACCOUNT} --region ${AWS_REGION} \
    && pip3 install -r requirements.txt --target "${LAMBDA_TASK_ROOT}"

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
CMD [ "tre_bagit_to_dri_sip.handler" ]