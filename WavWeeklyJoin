import java.io.*;
import java.util.*;
import java.nio.file.Files;
import javax.sound.sampled.AudioFileFormat;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import java.text.SimpleDateFormat;
import java.text.DateFormat;

// The MIT License (MIT)

// Copyright (c) [2014] [Jonathan J Mercer]

// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:

// The above copyright notice and this permission notice shall be included in all
// copies or substantial portions of the Software.

// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
// SOFTWARE.


/**
* @author: Jonathan J Mercer 
*
* What it does (TLDR): Concatinates multiple WAV files into one large one based on week created
*
* This code works on my 2013 MBP Retina runing OSX 10.9.2 
*
* Thanks to the following for their code in StackOverflow
* -James Van Huis: merging audio files
* -Ralph: week number of the year
*
* @Requirements:
* -Bit-rate and sample rate of WAV files must be exactly the same
* -There must be an "intro.wav" inside the "INPUT" folder
* -Path of input and output folders
* -Name of wav files must be "yyyy-mm-dd....wav"
*/
class WavWeeklyJoin {

	//You can change these based on your system 
	private static final String INPUT_PATH = "/Users/Odin/Desktop/testfolder";
	private static final String OUTOUT_PATH = "/Users/Odin/Desktop";
	private static final String INTRO = "intro.wav";

	/**
	* Contains the main logic of the code. Calls a bunch of helper functions
	*/
    public static void main(String[] args) {
    	try {
    		//Simple counters
	    	int weekNum = -1;
	    	int previousWeekNum = -1;
	    	int yearNum = -1;
	    	int previousYearNum = -1;

	    	//Initialize the currentWav
	    	AudioInputStream currentWav = AudioSystem.getAudioInputStream(new File(INPUT_PATH+"/"+INTRO));

	    	//Make a list of all items in the folder and itterate through them
	        File folder = new File(INPUT_PATH);
			File[] listOfFiles = folder.listFiles();
			for (File file : listOfFiles) {

			    if (file.isFile() && file.getName().contains(".wav") && file.getName().contains("-")) {
			        
			        weekNum = findWeekNum(file.getName());
			        yearNum = findYearNum(file.getName());

			   		//Checks if weeknum has changed
			        if(weekNum != previousWeekNum) {
			        	
			        	if(previousWeekNum > 0) {
			        		System.out.println("<-- Writing WAV with year "+previousYearNum+" and week " + previousWeekNum);
				        	writeWav(currentWav,previousWeekNum,previousYearNum);
			        	}

			        	System.out.println("<-- Creating new WAV");
			        	currentWav = AudioSystem.getAudioInputStream(new File(INPUT_PATH+"/"+INTRO));
			        	currentWav = appendWav(currentWav,file.getName());
			        	
			        	previousWeekNum = weekNum;
			        	previousYearNum = yearNum;

			        } else {

			        	System.out.println("<-- Appending WAV");
			        	currentWav = appendWav(currentWav,file.getName());
			        }
			    }
			}

			System.out.println("<-- Writing final WAV");
			writeWav(currentWav,previousWeekNum,yearNum);

		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	* Find the week number based on date
	* @param String date
	* @return int week number
	*/
	public static int findWeekNum(String date) {
		try{
			//Strips out just the date from the file name
			String shortDate = date.substring(0,10);

			DateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		    Date parsedShortDate = sdf.parse(shortDate);

		    Calendar calUs = Calendar.getInstance(Locale.US);       
		    calUs.setTime(parsedShortDate);

		    return calUs.get( Calendar.WEEK_OF_YEAR );

		} catch (Exception e) {
			System.out.println("Error");
		}
		return -1;//note: this return value is not handled properly on the other end
	}

	/**
	* Find the year number based on date
	* @param String date
	* @return int year number
	*/
	public static int findYearNum(String date) {
		return Integer.parseInt(date.substring(0,4));
	}

	/**
	* Appends two WAV files
	* @param AudioInputStream currentWav: the main file
	* @param String appendName: a name of a WAV file that exists in the INPUT_PATH
	* @return AudioInputStream appended WAV files
	*/
	public static AudioInputStream appendWav(AudioInputStream currentWav, String appendName) {
		try{
		
		AudioInputStream toAppend = AudioSystem.getAudioInputStream(new File(INPUT_PATH+"/"+appendName));
		AudioInputStream appendedFiles = 
                            new AudioInputStream(
                                new SequenceInputStream(currentWav, toAppend),     
                                currentWav.getFormat(), 
                                currentWav.getFrameLength() + toAppend.getFrameLength());

        return appendedFiles;

	    } catch (Exception e) {
	    	e.printStackTrace();
	    }
	    //Note, I'm not catching this null on the return. 
	    return null;
	}

	/**
	* Writes the appended WAV files into the output folder
	* @param AudioInputStream wav: the file we want to write
	* @param int week: 
	* @param int year: 
	*/
	public static void writeWav(AudioInputStream wav, int week, int year) {
	    try {
	    	if(week < 10) {
	    		//Prepend a 0 infront of the number
			    AudioSystem.write(wav, 
                        AudioFileFormat.Type.WAVE, 
                        new File(OUTOUT_PATH+"/"+year+"-0"+week+".wav"));	
	    	} else {
	    		AudioSystem.write(wav, 
                        AudioFileFormat.Type.WAVE, 
                        new File(OUTOUT_PATH+"/"+year+"-"+week+".wav"));
	    	}
	    } catch (Exception e) {
	    	System.out.println("Error in writeWav");
		    e.printStackTrace();
	    }
	}

}
