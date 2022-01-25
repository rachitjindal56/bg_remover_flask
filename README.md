# Image Background Remover Web APP

A Deep Learning object detection based web app that removes the background of the images using a Neural Network model. Created on Flask and deployed on Herkou, this app uses a DeepLabV3 Xception model for object detection and 
image processing techniques to remove the background.

# App

### 1) Creation of Static upload folder for saving the final image files for download
    
    UPLOAD_FOLDER = 'static/uploads/'
    app.secret_key = "secret key"
    app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
    
### 2) Model Loading

    change_bg = alter_bg()
    change_bg.load_pascalvoc_model("static/deeplabv3_xception_tf_dim_ordering_tf_kernels.h5")

### 3) Input functions and file background removal:

    @app.route("/",methods=['POST','GET'])
    def image_upload():
        filename = ''
        if request.method == "POST":
            image = request.files["image"]

            filename = secure_filename(image.filename)
            image.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            flash('Image successfully uploaded and displayed below')
            
            output = change_bg.color_bg('static/uploads/'+filename, colors = (255, 255, 255))
            cv2.imwrite('static/output/'+filename,output)
            
            return redirect("/display/"+filename)
        
        return render_template("home.html",filename=filename)
        
### 4) Display and Download functions:

    @app.route("/display/<filename>")
    def display(filename):
        return render_template('display_image.html',filename="output/"+filename)

    @app.route("/download/<path:fx>",methods = ['GET','POST'])
    def download(fx):
        return send_file('static/'+fx,as_attachment=True)
  
# Requirements

### Refer to requirements.txt file.

absl-py==0.13.0
astunparse==1.6.3
attrs==21.2.0
cachetools==4.2.2
certifi==2021.5.30
charset-normalizer==2.0.4
clang==5.0
click==8.0.1
colorama==0.4.4
cycler==0.10.0
Flask==2.0.1
flatbuffers==1.12
gast==0.4.0
google-auth==1.35.0
google-auth-oauthlib==0.4.5
google-pasta==0.2.0
grpcio==1.39.0
gunicorn==20.1.0
h5py==3.1.0
idna==3.2
imageio==2.9.0
imantics==0.1.12
imgaug==0.4.0
itsdangerous==2.0.1
Jinja2==3.0.1
jsonschema==3.2.0
keras==2.6.0
Keras-Preprocessing==1.1.2
kiwisolver==1.3.1
labelme2coco==0.1.2
lxml==4.6.3
Markdown==3.3.4
MarkupSafe==2.0.1
matplotlib==3.4.3
networkx==2.6.2
numpy==1.19.5
oauthlib==3.1.1
opencv-python==4.5.3.56
opt-einsum==3.3.0
Pillow==8.3.1
pixellib==0.6.6
protobuf==3.17.3
pyasn1==0.4.8
pyasn1-modules==0.2.8
pyparsing==2.4.7
pyrsistent==0.18.0
python-dateutil==2.8.2
PyWavelets==1.1.1
requests==2.26.0
requests-oauthlib==1.3.0
rsa==4.7.2
scikit-image==0.18.3
scipy==1.7.1
Shapely==1.7.1
six==1.15.0
tensorboard==2.6.0
tensorboard-data-server==0.6.1
tensorboard-plugin-wit==1.8.0
tensorflow==2.6.0
tensorflow-estimator==2.6.0
termcolor==1.1.0
tifffile==2021.8.8
typing-extensions==3.7.4.3
urllib3==1.26.6
Werkzeug==2.0.1
wrapt==1.12.1
xmljson==0.2.1

## Authors

- [Rachit R Jindal](https://github.com/rachitjindal56)


## Tech Stack

**Client:** HTML, Heroku

**Server:** Python, cv2,,Flask,Tensorflow, Heroku,

