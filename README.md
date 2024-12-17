import org.tensorflow.lite.Interpreter;
import java.io.File;
import java.nio.MappedByteBuffer;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class PestDetectionSystem {
    private Interpreter tflite;
    private static final String MODEL_PATH = "path/to/model.tflite"; // TensorFlow Lite model path
    private static final String CAMERA_PATH = "/path/to/camera/images/"; // Rover's camera image folder

    public PestDetectionSystem() {
        try {
            // Load the TensorFlow Lite model
            MappedByteBuffer model = loadModel(MODEL_PATH);
            tflite = new Interpreter(model);
        } catch (Exception e) {
            System.err.println("Error loading model: " + e.getMessage());
        }
    }

    // Method to load the TensorFlow Lite model
    private MappedByteBuffer loadModel(String modelPath) throws Exception {
        File file = new File(modelPath);
        return Files.newByteChannel(Paths.get(file.getPath())).map(
            FileChannel.MapMode.READ_ONLY,
            0,
            file.length()
        );
    }

    // Process image through the AI model
    public String detectPest(String imagePath) {
        // Load image (placeholder for actual image preprocessing)
        float[][] inputImage = preprocessImage(imagePath);

        // Prepare output array for predictions
        float[][] output = new float[1][NUM_CLASSES]; // Adjust NUM_CLASSES based on your model

        // Run inference
        tflite.run(inputImage, output);

        // Analyze output to determine pest type
        return interpretResults(output);
    }

    // Placeholder for image preprocessing (actual code will depend on your library)
    private float[][] preprocessImage(String imagePath) {
        // Load and normalize image data
        return new float[1][224 * 224 * 3]; // Placeholder for processed image
    }

    // Interpret model output to classify pests
    private String interpretResults(float[][] output) {
        int classIndex = 0;
        float maxScore = 0;

        // Find the class with the highest probability
        for (int i = 0; i < output[0].length; i++) {
            if (output[0][i] > maxScore) {
                maxScore = output[0][i];
                classIndex = i;
            }
        }

        // Return pest type based on class index
        String[] pestClasses = {"No Pest", "Mango Mealy Bug", "Aphid", "Whitefly"};
        return pestClasses[classIndex];
    }

    // Main function for testing
    public static void main(String[] args) {
        PestDetectionSystem pestSystem = new PestDetectionSystem();

        // Test with images captured by the rover
        File folder = new File(CAMERA_PATH);
        File[] images = folder.listFiles();

        if (images != null) {
            for (File image : images) {
                String result = pestSystem.detectPest(image.getPath());
                System.out.println("Image: " + image.getName() + " - Detection: " + result);
            }
        } else {
            System.out.println("No images found in camera path.");
        }
    }
}

  

