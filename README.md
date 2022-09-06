# FinalProject-AI

Para el proyecto trabajaremos utilizando la deteccion y el seguimiento de objetos la cual vamos a implementar el archivo Tracker.py 
para el seguimiento y el archivo Main.py contendra los parametros que tendran el video.

1. Implementar funciones para seguimiento
2. Implementar parametros para la deteccion de objetos


# Videos de Youtube

Link de la explicacion y ejecucion del código [Video](https://www.youtube.com/watch?v=MYsncz0bwuc&t=161s)
Link del manual de usuario [Video](https://www.youtube.com/watch?v=9PG-DLEwOSQ&t=80s)

# Integrantes 
Chávez Dustin
Guañuna Steven 
Pinanjota Kevin

## 1. TRACKER
Importamos las librerias a utilizar e implementamos las funciones

![image](https://user-images.githubusercontent.com/74762981/188532815-73852ec6-7677-4249-a14f-188ac9b5a635.png)


Almacenamos las posiciones centrales de los objetos

![image](https://user-images.githubusercontent.com/74762981/188534008-7b4605ff-cbed-47b3-8b08-9bad2d0017aa.png)

Mantenga el conteo de las identificaciones
cada vez que se detecta una nueva identificación de objeto, el conteo aumentará en uno

![image](https://user-images.githubusercontent.com/74762981/188534265-945063b2-63db-4951-9e18-662c7c88c77b.png)

![image](https://user-images.githubusercontent.com/74762981/188534393-a9c72eb5-f8d3-401a-9b85-c36cd80945a4.png)

Cajas de objetos e identificaciones

![image](https://user-images.githubusercontent.com/74762981/188534433-bee11386-c509-433e-b7c3-3cf8b61836d8.png)

Obtenemos el punto central del nuevo objeto

![image](https://user-images.githubusercontent.com/74762981/188534506-68d99b19-f47e-4e86-a275-58eba9a3af93.png)

Averigüe si ese objeto ya fue detectado

![image](https://user-images.githubusercontent.com/74762981/188534563-ec3d40ec-20c7-401e-a78d-82ae1c7cd053.png)

Se detecta un nuevo objeto, asignamos la ID a ese objeto

![image](https://user-images.githubusercontent.com/74762981/188534765-a14ba18b-5cbf-4d0e-84a7-18cee04c3f26.png)

Limpie el diccionario por puntos centrales para eliminar IDS que ya no se usan

![image](https://user-images.githubusercontent.com/74762981/188534830-f3619acc-7866-4b5b-9882-9fa5380e03d7.png)

Actualizar diccionario con ID no utilizados eliminados

![image](https://user-images.githubusercontent.com/74762981/188534862-b4a49d6c-2f7c-4082-84c9-6c26fc459b1a.png)

## 1. TRACKER - Codigo Completo

class EuclideanDistTracker:
    def __init__(self):
        self.center_points = {}
        self.id_count = 0

    def update(self, objects_rect):
        objects_bbs_ids = []

        for rect in objects_rect:
            x, y, w, h = rect
            cx = (x + x + w) // 2
            cy = (y + y + h) // 2

            same_object_detected = False
            for id, pt in self.center_points.items():
                dist = math.hypot(cx - pt[0], cy - pt[1])

                if dist < 25:
                    self.center_points[id] = (cx, cy)
                    print(self.center_points)
                    objects_bbs_ids.append([x, y, w, h, id])
                    same_object_detected = True
                    break

            if same_object_detected is False:
                self.center_points[self.id_count] = (cx, cy)
                objects_bbs_ids.append([x, y, w, h, self.id_count])
                self.id_count += 1

        new_center_points = {}
        for obj_bb_id in objects_bbs_ids:
            _, _, _, _, object_id = obj_bb_id
            center = self.center_points[object_id]
            new_center_points[object_id] = center

        self.center_points = new_center_points.copy()
        return objects_bbs_ids


## 2. MAIN
Importamos la libreria cv2 que es el opencv que utilzaremos para la deteccion de los objetos, motos y carros

![image](https://user-images.githubusercontent.com/74762981/188537700-321a75bc-5f6e-44bc-b29f-97e57fc1dcec.png)

Creamos objeto rastreador

![image](https://user-images.githubusercontent.com/74762981/188537804-0d662e40-8f23-492b-ad48-d00ea9108824.png)

Iniciamos la ruta para capturar los fotogramas del video

![image](https://user-images.githubusercontent.com/74762981/188537963-6917bf43-5ccb-48da-bbe3-b99c42f7bf53.png)

Detección de objetos desde la cámara estable

![image](https://user-images.githubusercontent.com/74762981/188538019-1cbc7956-86f7-4712-9698-7b13c548311a.png)

Iniciamos un ciclo mientras es verdadero considerando que es un video

![image](https://user-images.githubusercontent.com/74762981/188538062-77878631-4a0c-4738-b50f-68a7ebf95158.png)

Extraer región de interés
altura y ancho

![image](https://user-images.githubusercontent.com/74762981/188538184-1936bf57-d91e-47a3-8896-e57d2a216122.png)

## Detección de objetos 

Extraemos en timepo real con una mascara

![image](https://user-images.githubusercontent.com/74762981/188538332-0021cfb6-8ea8-4fbe-84a8-1eff05b1b163.png)

cuando existen sombras tb son detectadas como objetos lo cual vamos a limpiar
la mascara cuando los valores de pixeles sean menores a 254

![image](https://user-images.githubusercontent.com/74762981/188538375-4b5997c0-f5d4-4977-aacd-3a5a7c6a2725.png)


Extraemos las coordenadas y tulizamos la funcion OPEN que es CV2.FIND 
para encontrar los contornos de los limites de estos objetos blancos en la masccara

![image](https://user-images.githubusercontent.com/74762981/188538426-59816636-18d4-47bc-9930-31813472d152.png)

se crea una matriz :esta es una matriz vacia ,y cada vez que encontramos las cajas,las 
guardamos dentro de la matriz

![image](https://user-images.githubusercontent.com/74762981/188538469-9a6378cd-9965-4edb-b7ea-65fd7adcabff.png)

Calcula el área y elimina elementos pequeños

![image](https://user-images.githubusercontent.com/74762981/188538518-45c0aad4-0b9e-4264-8ae2-2dbe79584e16.png)

ponemos una condicion, si el area es menor no hace nada y si es mayor que dibuje el contorno

![image](https://user-images.githubusercontent.com/74762981/188538551-a3be8b07-210f-414d-bf40-1f85ed7b236e.png)

 En lugar de dibujar los contornos vamos a extraer el caudro 
 que rodea cada objeto que sea detectado
 
 ![image](https://user-images.githubusercontent.com/74762981/188538598-b35450ed-c5be-43fc-a6ef-8538cf37c43f.png)

aqui se va a ir agregando ala final de la lista

![image](https://user-images.githubusercontent.com/74762981/188538645-ed9d547f-0e61-48b1-838d-04de350d382c.png)

## Seguimiento de objetos

Ahora necesitamos tener detecciones donde los IDS de las cajas son iguales a la actualizacion del punto del rastreador

![image](https://user-images.githubusercontent.com/74762981/188539573-c3e0016b-2e7d-4195-a8fd-0112b250a0a8.png)

Identificacion de la caja en las IDENTIFICACIONES DE LAS CAJAS

![image](https://user-images.githubusercontent.com/74762981/188541144-69cd3279-f1ec-4e6f-bc5f-424bcc69655f.png)

extraemos la altura y ancho para la identificacion de la caja

![image](https://user-images.githubusercontent.com/74762981/188541201-6c5c4a71-d957-4939-a6d0-aff546829693.png)

ponemos -15 solo para colocar una pequena distancia del objeto y
el color de los numeros en azul con tamano #2 y grosor #2

![image](https://user-images.githubusercontent.com/74762981/188541254-a025f239-e5d8-4706-a397-228835b9d92a.png)

el color de los contornos en verde con grosor # 3

![image](https://user-images.githubusercontent.com/74762981/188541299-7396caaf-ce79-494d-9b46-fb0e8cd118e6.png)

gracias a los IDS somos capaces de detectar y para contar correctamente los objetos que van pasando

![image](https://user-images.githubusercontent.com/74762981/188541334-2db5084e-2d51-4b2d-9f14-369f61f55cd3.png)

La función waitKey()espera un evento clave para un "retraso" (aquí, 30 milisegundos).

![image](https://user-images.githubusercontent.com/74762981/188541359-1506a811-05db-4606-93a7-7b96ee58a10e.png)

## MAIN - Codigo completo

import cv2
from tracker import *

tracker = EuclideanDistTracker()


cap = cv2.VideoCapture("highway1.mp4")  

object_detector = cv2.createBackgroundSubtractorMOG2(history=100, varThreshold=40)


while True: 
    ret, frame = cap.read()
    height, width, _ = frame.shape
       
    roi = frame[100: 500,800: 1000]

    # 1. Detección de objetos 


    mask = object_detector.apply(roi)   

    _, mask = cv2.threshold(mask, 254, 255, cv2.THRESH_BINARY)

    
    contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    detections = []
    for cnt in contours:


        area = cv2.contourArea(cnt)

        if area > 1000:

            x, y, w, h = cv2.boundingRect(cnt)


            detections.append([x, y, w, h])

    # 2. Seguimiento de objetos

    boxes_ids = tracker.update(detections)


    for box_id in boxes_ids:


        x, y, w, h, id = box_id

        
        cv2.putText(roi, str(id), (x, y - 15), cv2.FONT_HERSHEY_PLAIN, 2, (255, 0, 0), 2)

        #   el color de los contornos en verde con grosor # 3                                        color y grosor
        
        
        cv2.rectangle(roi, (x, y), (x + w, y + h), (0, 255, 0), 3)

 
    cv2.imshow("roi", roi)
    cv2.imshow("Frame", frame)
    cv2.imshow("Mask", mask)


    key = cv2.waitKey(30)
    if key == 27:
        break

cap.release()
cv2.destroyAllWindows()

# Resultados
Mascara

![image](https://user-images.githubusercontent.com/74762981/188542099-58e87526-112e-4176-b431-90c20f1e3b0d.png)

Zona de Interes

![image](https://user-images.githubusercontent.com/74762981/188542141-f4ab31d1-3514-4ac9-a261-33dd1a32ac4d.png)


FRAME

![image](https://user-images.githubusercontent.com/74762981/188542321-5335dc27-8bfc-4cab-b4bd-3529b069c6f0.png)

