# speech-to-text
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Mon Sep  7 23:58:00 2020

@author: chitra
"""
import pyaudio
import wave
import speech_recognition as sr
import tkinter as Tk



filename = "recorded.wav"

def runRecord(inputTime, T):
    # the file name output you want to record into
    
    # set the chunk size of 1024 samples
    chunk = 1024
    # sample format
    FORMAT = pyaudio.paInt16
    # mono, change to 2 if you want stereo
    channels = 1
    # 44100 samples per second
    sample_rate = 44100
    record_seconds = inputTime
    # initialize PyAudio object
    p = pyaudio.PyAudio()
    # open stream object as input & output
    stream = p.open(format=FORMAT,
                    channels=channels,
                    rate=sample_rate,
                    input=True,
                    output=True,
                    frames_per_buffer=chunk)
    frames = []
    print("Recording...")
    for i in range(int(44100 / chunk * record_seconds)):
        data = stream.read(chunk)
        # if you want to hear your voice while recording
        # stream.write(data)
        frames.append(data)
    print("Finished recording.")
    # stop and close stream
    stream.stop_stream()
    stream.close()
    # terminate pyaudio object
    p.terminate()
    # save audio file
    # open the file in 'write bytes' mode
    wf = wave.open(filename, "wb")
    # set the channels
    wf.setnchannels(channels)
    # set the sample format
    wf.setsampwidth(p.get_sample_size(FORMAT))
    # set the sample rate
    wf.setframerate(sample_rate)
    # write the frames as bytes
    wf.writeframes(b"".join(frames))
    # close the file
    wf.close()

    r = sr.Recognizer()

    # open the file
    with sr.AudioFile(filename) as source:
        # listen for the data (load audio to memory)
        audio_data = r.record(source)
        # recognize (convert from speech to text)
        text = r.recognize_google(audio_data)
        print(text)
    
    T.insert(Tk.END, text)



top = Tk.Tk()


T = Tk.Text(top, height=2, width=30)
T.pack()
b = Tk.Button(top,text = "2 Second", command = lambda: runRecord(2, T))  
b.pack()  

b2 = Tk.Button(top,text = "4 Second", command = lambda:  runRecord(4, T))  
b2.pack()  


b3 = Tk.Button(top,text = "7 Second", command = lambda: runRecord(7, T))  
b3.pack()  
  
b4 = Tk.Button(top,text = "10 Second", command =lambda:  runRecord(10, T))  
b4.pack()




  

top.mainloop() 









#Starting the converting process.





