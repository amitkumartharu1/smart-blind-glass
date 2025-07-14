# smart-blind-glass
const int trigPin = 9;
const int echoPin = 10;
const int motorPin = 6; // connected directly to vibration motor

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(motorPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo time
  duration = pulseIn(echoPin, HIGH);

  // Convert to distance (in cm)
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Control vibration intensity using PWM (0-255)
  if (distance <= 100) {
    int intensity = map(distance, 100, 5, 0, 255); // Closer = stronger vibration
    intensity = constrain(intensity, 0, 255);
    analogWrite(motorPin, intensity);

    // Print PWM intensity
    Serial.print("PWM Intensity: ");
    Serial.println(intensity);
  } else {
    analogWrite(motorPin, 0); // No vibration
    Serial.println("PWM Intensity: 0");
  }

  delay(100);
}
