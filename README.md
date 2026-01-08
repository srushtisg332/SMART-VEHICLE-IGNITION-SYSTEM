
# Smart Vehicle Ignition System Using Multi-Level Authentication

## Project Overview
The Smart Vehicle Ignition System is a secure, keyless vehicle access solution designed to prevent unauthorized ignition. The system replaces traditional mechanical keys with multi-level electronic authentication using RFID, biometric fingerprint verification, and face recognition. Ignition is enabled only when valid credentials are successfully verified, significantly enhancing vehicle security.

## Objective
The primary objective of this project is to improve vehicle safety by implementing a reliable and secure ignition control mechanism using embedded systems and electronic authentication methods.

## Technologies Used
- Embedded C  
- Arduino IDE  
- Microcontroller (Arduino / ESP32)  
- RFID Module  
- Biometric Fingerprint Sensor  
- Face Authentication Module  
- Communication Interfaces (UART, SPI)  

## System Working
- User presents an RFID card or provides biometric input  
- The microcontroller reads authentication data from the modules  
- Credentials are verified against stored authorized data  
- If authentication is valid, the ignition system is enabled  
- If authentication fails, the ignition remains locked  

## System Architecture
1. Input modules (RFID, biometric, face recognition) capture user credentials  
2. Microcontroller processes and verifies authentication data  
3. Decision logic determines ignition access  
4. Ignition control output enables or disables vehicle ignition  

## My Contribution
- Developed Embedded C code for authentication logic  
- Interfaced RFID, biometric, and face recognition modules  
- Implemented ignition enable and lock control logic  
- Debugged module communication and response delays  
- Tested system reliability under multiple authentication scenarios  

## Challenges Faced
- Delays in biometric sensor response time  
- Synchronization issues between multiple authentication modules  
- Handling communication latency between peripherals  

## Learning Outcomes
- Multi-module hardware integration  
- Real-time decision-making using embedded systems  
- Secure system design for safety-critical applications  
- UART and SPI communication handling  
- Debugging and optimization of embedded applications  

## Applications
- Two-wheelers and four-wheelers  
- Fleet and commercial vehicle security  
- Smart transportation systems  
- Anti-theft vehicle solutions  

## Future Improvements
- Mobile application-based authentication  
- GPS tracking and real-time theft alerts  
- Cloud-based access and authentication logs  
- Integration with CAN bus for modern vehicles  
- Improved biometric response and accuracy  

## License
This project is licensed under the MIT License.

## Code

            Dataset collection.py 
            
            import cv2 
            import random 
            cam=cv2.VideoCapture(0) 
            cascade=cv2.CascadeClassifier("haarcascade_frontalface_default.xml") 
            while True: 
            flag,frame=cam.read() 
            test_image_gray=cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY) 
            faces=cascade.detectMultiScale(test_image_gray,1.1,5) 
            print(faces) 
            if len(faces) >0 : 
            x,y,w,h=faces[0] 
            cv2.rectangle(frame,(x,y),(x+w,y+h),(255,0,0),2) 
            cv2.imshow("testimage",frame) 
            k=cv2.waitKey(2) 
            if k==ord('q'): 
            break 
            if k==ord('s'): 
            roi=frame[y:y+h,x:x+w] # face cropping 
            roi=cv2.resize(roi,(300,300)) 
            n=random.randint(1,200) 
            filename=f"./dataset/0/person{n}.jpg" 
            cv2.imwrite(filename,roi) 
            cam.release() 
            
            Training.py 
            
            ''' 
            in this file we extract features of 
            dataset and train the model using LBPH 
            ''' 
            from os import listdir 
            import cv2 
            import numpy as np 
            recog=cv2.face.LBPHFaceRecognizer_create() 
            root_dir="./dataset" 
            features=[] 
            label=[] 
            i=0 
            for subfolder in listdir(root_dir): 
            folder_path=f"{root_dir}/{subfolder}" 
            print(f"-------{folder_path}--------- ") 
            for file in listdir(folder_path): 
            file_path=f"{folder_path}/{file}" 
            print(file_path) 
            image=cv2.imread(file_path,0) 
            features.append(image) 
            label.append(i) 
            # print(image) 
            # cv2.imshow("img",image) 
            # cv2.waitKey() 
            i+=1 
            
            Prediction.py 
            
            import cv2 
            import numpy as np 
            from apidemo import insert 
            recog=cv2.face.LBPHFaceRecognizer_create() 
            cam=cv2.VideoCapture(0) 
            names={ 
            0:"K.B.raj", 
            1:"Suchitra", 
            2:"Shwetha" 
            } 
            recog.read("facemodel.yml") 
            cascade=cv2.CascadeClassifier("haarcascade_frontalface_default.xml") 
            #above line reads the model 
            while True: 
            # test_image=cv2.imread("./dataset/0/person2.jpg") 
            flag,test_image=cam.read() 
            if flag: 
            gray=cv2.cvtColor(test_image,cv2.COLOR_BGR2GRAY) 
            faces=cascade.detectMultiScale(gray,1.1,5) 
            Dept. of E&CE, BIET, Davangere. 
            Page 39 
            SMART VEHICLE IGNITION SYSTEM 
            if len(faces) > 0: 
            x,y,w,h=faces[0] 
            cv2.rectangle(test_image,(x,y),(x+w,y+h),(255,0,0),2) 
            roi=gray[y:y+h,x:x+w] 
            roi=cv2.resize(roi,(300,300)) 
            id,confi=recog.predict(roi) 
            result=f"id : {id} , confi : {confi}" 
            msg="Welcome" 
            if confi < 30: 
            detected_name=names[id] 
            flag=insert(detected_name) 
            if flag=="1": 
            msg="Attendance for the today already recorded" 
            else: 
            msg="Attendance taken,Thank you" 
            else: 
            detected_name="Un known" 
            text = detected_name 
            font = cv2.FONT_HERSHEY_SIMPLEX 
            position = (50, 50) # (x, y) coordinates 
            font_scale = 1 
            font_color = (0, 0, 255) # Red color in BGR 
            thickness = 2 
            line_type = cv2.LINE_AA 
            # Put text on the image 
            cv2.putText(test_image, text, position, font, font_scale, font_color, thickness, line_type) 
            cv2.putText(test_image, msg, (50,100), font, font_scale,(255,0,0), thickness, line_type) 
            print(result) 
            cv2.imshow("Result of prediction",test_image) 
            k=cv2.waitKey(2) 
            if k==ord('q'): 
            break 
            cam.release()

## Conclusion
The Smart Vehicle Ignition System demonstrates an effective embedded security solution for modern vehicles by replacing traditional keys with multi-level electronic authentication. By integrating RFID, biometric, and face recognition technologies with a microcontroller-based control unit, the system ensures secure ignition access while reducing the risk of unauthorized vehicle usage. This project highlights the practical application of embedded systems in automotive security and intelligent transportation systems.

            
            
            
            
