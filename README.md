# FinalProject-AI
Para el proyecto trabajaremos utilizando la deteccion y el seguimiento de objetos la cual vamos a implementar el archivo Tracker.py 
para el seguimiento y el archivo Main.py contendra los parametros que tendran el video.

1. Implementar funciones para seguimiento
2. Implementar parametros para la deteccion de objetos


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



