using System.Collections;
using System.IO;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Windows;
using File = System.IO.File;

public class SearchGoogle : MonoBehaviour
{
    public string keyword;
    private string searchwebsite;
    string fileName = "MyFile.html";

    void Start()
    {
        searchwebsite = "https://www.google.com/search?q=" + keyword;
        Debug.Log(searchwebsite);
        StartCoroutine(GetText());
        Debug.Log("Coroutine wird gecallt");
    }

    IEnumerator GetText()
    {
        UnityWebRequest www = UnityWebRequest.Get(searchwebsite);
        yield return www.SendWebRequest();

        if (www.isNetworkError || www.isHttpError)
        {
            Debug.Log("Fehler:(");
            Debug.Log(www.error);
        }
        else
        {
            Debug.Log("CErfolg");
            // Show results as text
            Debug.Log(www.downloadHandler.text);

            if (File.Exists(fileName))
            {
                Debug.Log(fileName + " already exists.");
            }
            var sr = File.CreateText(fileName);
            sr.WriteLine(www.downloadHandler.text);
            sr.Close();

            //Printing it in a file to open it in my browser, not nessecary in the final build



            // Or retrieve results as binary data
            byte[] results = www.downloadHandler.data;
        }
    }
} 
