# Catch-Circle
catch the circle with your hands!

### 1. overview
   use the Mediapipe library to create an interactive game where the player can grab circles with their hand to increase their score. This game will utilize   
   real-time hand tracking to detect when the player's hand interacts with the circles displayed on the screen. The primary objective is to enhance the player's score 
   by successfully grabbing as many circles as possible within a given time frame.<br/>**※To use this code, you need to install the Mediapipe library.**

### 2. required library
   mediapipe
   ``` python
   pip install mediapipe
   ```
   
   openCV
   ``` python
   pip install opencv-python
   ```

### 3. important detail of code
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
   checking if a circle is grabbed.

   ``` python
def is_fist(hand_landmarks):
    if hand_landmarks:
        for hand_landmark in hand_landmarks:
            thumb_tip = hand_landmark.landmark[4]
            index_tip = hand_landmark.landmark[8]
            middle_tip = hand_landmark.landmark[12]
            ring_tip = hand_landmark.landmark[16]
            pinky_tip = hand_landmark.landmark[20]

            tips = [thumb_tip, index_tip, middle_tip, ring_tip, pinky_tip]
            tip_positions = [(int(tip.x * 640), int(tip.y * 480)) for tip in tips]

            distances = [((tip_positions[0][0] - tip[0]) ** 2 + (tip_positions[0][1] - tip[1]) ** 2) ** 0.5 for tip in tip_positions[1:]]
            return all(distance < 50 for distance in distances)  # 모든 손가락 끝이 서로 가까이 모여있는지 확인
    return False
   ```
   Checking if a fist is clenched.

### 4. real play video
<figure class="half">  <a href="link"><img src="https://github.com/b0v0d/Catch-Circle/assets/162780235/ee1a5c16-370d-4572-be8d-969adf97a20f" width=49%></a>  <a href="link"><img src="https://github.com/b0v0d/Catch-Circle/assets/162780235/5a7e3ba7-7304-48e8-bef0-749bb618a0f6" width=49%></a></figure>
