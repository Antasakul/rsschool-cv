# CV Anastasiya Kulik
## Contact information
email: nastakulik964@gmail.com

discord: Anastasiya(@Antasakul)
## About me
A member of the university student council, participated in charity events. I strive to obtain higher education, I am considering the possibility of studying for a master's degree. I have a high learning ability, I quickly learn new information and apply it in practice.
## Skills
1. knowledge of the c++ programming language
2. the methodology of OOP programming
3. еnglish (intermediate)
## Education 
I study at the Belarusian State University of Informatics and Radioelectronics, specializing in automated information processing systems.
## Projects
The first draft of the CV.
## Coding
**The task.**
Define a TextFile class to represent text files. Define the following functions in the class:
1. ***Open()*** – open a file. The function should create a buffer and read the file contents into it;
2. ***AddText()*** – add text to the end of the file;
3. ***PasteText()*** – paste text into an arbitrary location;
4. ***Save()*** – save a file. The function should have a parameter – a file name with a default value of NULL. If the default value is used, the file is saved with the name that was specified when it was opened;
5. ***Print()*** – display the file contents on the screen.

**Code.**
```
#include <iostream>
#include <cstring>
using namespace std;
class TextFile
{
private:
    char name[256]; 
    int length;     
    char* buff;     
public:
    TextFile()
    {
        name[0] = '\0';
        length = 0;
        buff = nullptr;
    }
    TextFile(const char* fileName)
    {
        strcpy_s(name, sizeof(name), fileName);
        length = 0;
        buff = nullptr;
    }
    ~TextFile()
    {
        delete[] buff;   
    }
    TextFile(TextFile& copy)
    {
       strcpy_s(name, sizeof(name), copy.name);
        length = copy.length;
        buff = new char[length + 1];
        strncpy_s(buff, length+1, copy.buff, length);
        buff = nullptr;
    }
    bool open()
    {
        FILE* file;
        if (fopen_s(&file, name, "rb") != 0)
        {
            printf("Ошибка при открытии файла: %s\n", name);
            return false;
        }
        fseek(file, 0, SEEK_END);
        length = ftell(file);
        fseek(file, 0, SEEK_SET);
        buff = new char[length + 1];
        fread(buff, sizeof(char), length, file);
        buff[length] = '\0';
        fclose(file);
        return true;
    }
    void addtext(const char* text)
    {
        int textLength = strlen(text);
        char* newBuff = new char[length + textLength + 1];
        strcpy_s(newBuff, length + textLength + 1, buff);
        strcat_s(newBuff, length + textLength + 1, text);
        delete[] buff;
        buff = newBuff;
        length += textLength;
    }
    void pastetext(const char* text, int position)
    {
        int textLength = strlen(text);
        char* newBuff = new char[length + textLength + 1];
        strncpy_s(newBuff, length + textLength + 1, buff, position);
        strncat_s(newBuff, length + textLength + 1, text, textLength);
        strncat_s(newBuff, length + textLength + 1, buff + position, length - position);
        delete[] buff;
        buff = newBuff;
        length += textLength;
    }
    void save(const char* filename = nullptr)
    {
        const char* saveFilename = (filename == nullptr) ? name : filename;
        FILE* file;
        if (fopen_s(&file, saveFilename, "wb") != 0)
        {
            printf("Ошибка при открытии файла:\n");
            return;
        }
        if (fwrite(buff, sizeof(char), length, file) != length)
        {
            printf("Ошибка при записи в файл: \n");
        }
        fclose(file);
    }
    void print()
    {
        printf("%s\n", buff);
    }
};
int main()
{
    setlocale(LC_ALL, "Russian");
    TextFile file1("example.txt");
    TextFile file2;
    TextFile file3(file1);
    file1.open();
    file1.addtext("Новый текст в конец.");
    file1.pastetext("Текст в произвольное место.", 10);
    file2.print();
    file3.open(); 
    file3.print(); 
    file1.save();
    file3.save("exam.txt");
    return 0;
}
```
