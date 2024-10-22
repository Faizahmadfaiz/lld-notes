Adapter Pattern 

The Adapter Pattern is a structural design pattern used to make two incompatible interfaces work together. It acts as a bridge between two interfaces, allowing objects with different interfaces to communicate.

Key Concepts:

	1.	Target Interface:
	•	The interface expected by the client, which the client interacts with.
	2.	Adaptee:
	•	The existing or legacy class that has a different interface but needs to be used by the client.
	3.	Adapter:
	•	Implements the target interface and wraps the adaptee. It translates requests from the client to the adaptee, enabling compatibility.
	4.	Client:
	•	Interacts only with the target interface, unaware of the adaptee or the adapter’s internal structure.

Structure:

	1.	Target Interface: Defines the standard method the client expects to call.
	2.	Adaptee: The class with the incompatible interface that needs adapting.
	3.	Adapter Class: Implements the target interface, internally using an instance of the Adaptee and translating the client’s calls.
	4.	Client: Uses the adapter as if it were the target interface, unaware of the internal translation.

Real-World Example: Media Player Example

Let’s create a scenario where you have an MP3 player but want to play MP4 files using it.

1. Target Interface: MediaPlayer

The client expects a method to play media.

interface MediaPlayer {
    void play(String fileType, String fileName);
}

2. Adaptee: MP3Player

This is the legacy system that can only play MP3 files.

class MP3Player {
    public void playMP3(String fileName) {
        System.out.println("Playing MP3 file: " + fileName);
    }
}

3. Adapter: MediaAdapter

This adapter will allow the MP3Player to play MP4 files by translating the call.

class MediaAdapter implements MediaPlayer {
    private MP3Player mp3Player;

    public MediaAdapter() {
        this.mp3Player = new MP3Player();  // Initialize the old MP3 player
    }

    @Override
    public void play(String fileType, String fileName) {
        if (fileType.equalsIgnoreCase("mp4")) {
            System.out.println("Converting MP4 to MP3...");
            mp3Player.playMP3(fileName);  // Convert and play the MP4 file as MP3
        }
    }
}

4. Client Code:

The client interacts with the MediaPlayer interface and is unaware of the conversion happening behind the scenes.

public class Main {
    public static void main(String[] args) {
        MediaPlayer player = new MediaAdapter();  // Using the adapter to play MP4 files
        
        // Client wants to play an MP4 file
        player.play("mp4", "myvideo.mp4");
    }
}

Output:

Converting MP4 to MP3...
Playing MP3 file: myvideo.mp4

Flow Explanation:

	1.	Client: Calls the play() method on MediaPlayer (adapter).
	2.	Adapter: Detects the file type, converts MP4 to MP3, and calls the playMP3() method from the MP3Player (adaptee).
	3.	Adaptee (MP3Player): Plays the MP4 file as an MP3.

Advantages of the Adapter Pattern:

	1.	Reusability: You can use legacy code (like MP3Player) without modifying it.
	2.	Flexibility: Adapters allow you to integrate new interfaces or systems easily.
	3.	Single Responsibility Principle: The adapter is responsible for translating requests between systems, promoting clean code separation.

Disadvantages:

	1.	Increased Complexity: Can add complexity if overused or for simple scenarios.
	2.	Performance Overhead: The translation process might introduce performance overhead, especially in performance-critical systems.

When to Use the Adapter Pattern:

	1.	Integrating with Legacy Systems: When you need to use an existing class (adaptee) that doesn’t match the expected interface.
	2.	Third-Party Integration: When you want to integrate with external libraries or systems that have different interfaces than your application.
	3.	Client-Interface Compatibility: When your client expects a specific interface, but the class you want to use has an incompatible interface.

Summary:

The Adapter Pattern provides a bridge between incompatible interfaces, making it possible for clients to use legacy systems or third-party libraries seamlessly. This pattern is especially useful in integrating legacy systems or external libraries, promoting flexibility and reusability in code.
