# udacity_people_counter_submission-
# Project Write-Up
command used to run the people counter app---you can check my workspace as well
python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m /home/workspace/ssd_mobilenet_v2_coco_2018_03_29/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3004/fac.ffm




## Explaining Custom Layers
I used a pretrained tensorflow model specifically SSD mobilenet which is fairly small and fast


STEPS I used to download and convert the SSD model 
 1-i DOWNLOAD THE SSD MODEL INTO MY LOCAL DIR 
 !wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz
 
 2-i UNZIP THE CONTENT 
 !tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz
 
 3-i USE THE COMMAND BELOW TO CONVERT IT 
 
python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json



## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were...

 1 Before conversion the raw mobile net model ;
  
  the size of original model is 266MB which consist of following contents:
checkpoint                  frozen_inference_graph.mapping      model.ckpt.index  pipeline.config
frozen_inference_graph.pb       model.ckpt.data-00000-of-00001  model.ckpt.meta   saved_model

 conversion into IR  we need two files  
 a- frozen_inference_graph.pb   
 b-pipeline.config


  
  2 After conversion :

after the conversion the size of intermediate representation is:
frozen_inference_graph.bin = 65M and frozen_inference_graph.xml = 110Kb.

due to the optimized Intermediate Representation of the model inference time is reduced by about 8 to 10 percent



## Assess Model Use Cases

Some of the potential use cases of the people counter app are...


The people counter app can be used as a solution to different problems involving surveillance in many sectors,below are a few:
1-The people counter app can be used  check meeting annd class attendance in Schools , Universities , and other institutions where people counting and attendance is of focus.
2-The people counter app can help stores check how well their stores are frequented and it can help them monitor the progress of the stores
3-The people counter app can be used to check for lareas where people violate social distancing rules .
4-The people counter app can be used to check the the number of people  who come to and leave the cinema and compare with the number of tickets sold 
5-The people counter app can be used to determine the products in stores that a lot of people prefer to buy
6-The people counter app can be used to regulate the number of people that can occupy a public bus to help ensure social distancing.
7-In military, the people counter app can be used as a counter terrorism tool operations tool through the use of drones


Each of these use cases would be useful because  surveillance and increase the added value service and profitability of a business

## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a
deployed edge model. The potential effects of each of these are as follows...

Lighting, model accuracy, and camera focal length/image size have different effects on a deployed edge model. The potential effects of each of these are as follows:-

for this application to be effective there should be a good amount of lightning present and artificial lightning can be provided during low light conditions.

I have used SSD Mobilenet V2 model which is has very less inference time along with pretty good accuracy. This model can predict big objects like humans in this case pretty easily with above 90% accuracy. This model is very suitable for detection on video streams due to its less inference time.

This app is created such that, at before doing the inference the video frame is resized according to the input shape to model. Standard frame size like (640 x 420) and (1280 x 720)
are recommended for optimal results.


