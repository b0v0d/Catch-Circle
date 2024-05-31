# Catch-Circle
catch the circle with your hands!

### 1. overview
   use the Mediapipe library to create an interactive game where the player can grab circles with their hand to increase their score. This game will utilize   
   real-time hand tracking to detect when the player's hand interacts with the circles displayed on the screen. The primary objective is to enhance the player's score 
   by successfully grabbing as many circles as possible within a given time frame.
   ※To use this code, you need to install the Mediapipe library.

### 2. required library
   mediapipe
   ``` python
   pip install mediapipe
   ```
   
   openCV
   ``` python
   pip install opencv-python
   ```

### 3. detail of code
   ``` python
   mp_hands = mp.solutions.hands
   hands = mp_hands.Hands(min_detection_confidence=0.7, min_tracking_confidence=0.7)
   mp_drawing = mp.solutions.drawing_utils
   ```
   initialize MediaPipe Hands

   ``` python
   def create_circle(color, radius):
       return {
           "pos": [random.randint(radius, 640-radius), random.randint(radius, 480-radius)],
           "radius": radius,
           "velocity": [random.choice([-5, 5]), random.choice([-5, 5])],
           "color": color,
           "score": 5 if color == (0, 255, 0) else 3 if color == (255, 0, 0) else -3
       }
   ```
   create circle and setting score of circle

   ``` python
   def is_circle_grabbed(hand_landmarks, circle):
    x, y = circle["pos"]
    radius = circle["radius"]
    if hand_landmarks:
        for hand_landmark in hand_landmarks:
            thumb_tip = hand_landmark.landmark[4]
            index_tip = hand_landmark.landmark[8]
            thumb_pos = (int(thumb_tip.x * 640), int(thumb_tip.y * 480))
            index_pos = (int(index_tip.x * 640), int(index_tip.y * 480))
            distance = ((thumb_pos[0] - index_pos[0]) ** 2 + (thumb_pos[1] - index_pos[1]) ** 2) ** 0.5
            if distance < radius and x - radius < index_pos[0] < x + radius and y - radius < index_pos[1] < y + radius:
                return True
    return False
   ```
