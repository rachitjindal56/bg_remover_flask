# bg_remover_flask
# App
from flask import Flask,flash, render_template, request, redirect
from flask.helpers import send_file
from werkzeug.utils import secure_filename
import pixellib
from pixellib.tune_bg import alter_bg
import cv2, os
import matplotlib.pyplot as plt

app = Flask(__name__)
UPLOAD_FOLDER = 'static/uploads/'
change_bg = alter_bg()
change_bg.load_pascalvoc_model("static/deeplabv3_xception_tf_dim_ordering_tf_kernels.h5")
 
app.secret_key = "secret key"
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

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
    
    
@app.route("/display/<filename>")
def display(filename):
    return render_template('display_image.html',filename="output/"+filename)

@app.route("/download/<path:fx>",methods = ['GET','POST'])
def download(fx):
    return send_file('static/'+fx,as_attachment=True)

if __name__ == '__main__':
    app.run(debug=True)
  
# Requirements
