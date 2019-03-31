# License plate detection

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/a3d989e67291492d9440b4f4727a4412)](https://app.codacy.com/app/Ramonrune/license-plate-detection?utm_source=github.com&utm_medium=referral&utm_content=Ramonrune/license-plate-detection&utm_campaign=Badge_Grade_Dashboard)

Detect and extract license plate.

## Method
This detection uses an haar-cascade file to detect license plate.<br>
Example<br>
<img src="http://answers.opencv.org/upfiles/14039682835002835.jpg"/><br>
```java
String xmlFile = "plate.xml";
CascadeClassifier classifier = new CascadeClassifier(xmlFile);

// Detecting the license plate in the snap
MatOfRect detections = new MatOfRect();

classifier.detectMultiScale(src, detections);
System.out.println(String.format("Detected %s faces", detections.toArray().length));

// Drawing boxes
int n = 0;
for (Rect rect : detections.toArray()) {

	Imgproc.rectangle(src, // where to draw the box
			new Point(rect.x, rect.y), // bottom left
			new Point(rect.x + rect.width, rect.y + rect.height), // top right
			new Scalar(0, 0, 255), 3 // RGB colour
	);

	Rect roi = new Rect(rect.x, rect.y, rect.width, rect.height);
	Mat cropped = new Mat(src, roi);

	Imgcodecs.imwrite("temp.jpg", cropped);

	// Encoding the image
	MatOfByte matOfByte = new MatOfByte();
	Imgcodecs.imencode(".jpg", cropped, matOfByte);

	// Storing the encoded Mat in a byte array
	byte[] byteArray = matOfByte.toArray();

	// Preparing the Buffered Image
	InputStream in = new ByteArrayInputStream(byteArray);
	try {
		BufferedImage bufImage = ImageIO.read(in);
		JLabel l = new JLabel("Imagem " + n);
		l.setSize(new Dimension(200, 100));

		l.setIcon(new ImageIcon(bufImage));
		listpossible.add(l);
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
	n++;

}
```
    
