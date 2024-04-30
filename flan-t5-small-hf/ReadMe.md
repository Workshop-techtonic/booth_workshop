To know about the model that you have chosen, please visit https://huggingface.co/google/flan-t5-small 

Steps to serve model using OpenShift AI:

1. Go to the tile and click Red Hat OpenShift AI
2. From the OpenShift AI dashboard, click Data Science Projects.
3. Create a data science project with the name myproject
4. Add Data connection using the following values
   
       Name: mystorage
       Accesskey: 
       Secretkey:
       Endpoint: https://s3.us-east-2.amazonaws.com/
       Region: us-east-2
       Bucket: ods-ci-wisdom
6. For serving the model, goto Models and model servers --> Single-model serving platform -> Deploy Model

     Use the values,

     Model name: mymodel

     Serving runtime: TGIS Standalone ServingRuntime for KServe

     Model framework: PyTorch

     Select Existing data connection --> select mystorage (that you have created in the previous step)

     Path: flan-t5-small/flan-t5-small-hf

7. Wait for Status to be green.
8. Copy the inference point. For example:  https://test-flang.apps.teckton.7ctq.s1.devshift.org
9. Query the model with the inference endpoint using grpc requests. 

    a. To query the model, clone the repository - https://github.com/IBM/text-generation-inference.git

    b. From the cloned directory execute the command,
        $  grpcurl -d '{"requests": [{"text":"What is the boiling temperature of water?"}]}' -H 'mm-model-id: <model id>' -insecure -proto text-generation-inference/proto/generation.proto <inference endpoint > fmaas.GenerationService/Generate
     
        For example,
        model id: mymodel
        inference endpoint: test-flang.apps.teckton.7ctq.s1.devshift.org:443
