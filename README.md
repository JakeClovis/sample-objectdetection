EN|[CN](README_cn.md)

The Object Detection application runs on the Atlas 200 DK or the AI acceleration cloud server and implements the inference function by using Faster R-CNN object detection network.

## Prerequisites<a name="en-us_topic_0167511792_section412314183119"></a>

Before using an open source application, ensure that:

-   MindSpore Studio has been installed. For details, see  [MindSpore Studio Installation Guide](https://www.huawei.com/minisite/ascend/en/filedetail_1.html).
-   The Atlas 200 DK developer board has been connected to MindSpore Studio, the cross compiler has been installed, the SD card has been prepared, and basic information has been configured. For details, see  [Atlas
200 DK User Guide](https://www.huawei.com/minisite/ascend/en/filedetail_2.html).

## Software Preparation<a name="en-us_topic_0167511792_section126492814528"></a>

Before running the application, obtain the source code package and configure the environment as follows.

1.  Obtain the source code package.

    Download all the code in the sample-objectdetection repository at  [https://github.com/Ascend/sample-objectdetection](https://github.com/Ascend/sample-objectdetection)  to any directory on Ubuntu Server where MindSpore Studio is located as the MindSpore Studio installation user, for example,  _/home/ascend/sample-objectdetection_.

2.  Log in to Ubuntu Server where MindSpore Studio is located as the MindSpore Studio installation user and set the environment variable  **DDK\_HOME**.

    **vim \~/.bashrc**

    Run the following commands to add the environment variables  **DDK\_HOME**  and  **LD\_LIBRARY\_PATH**  to the last line:

    **export DDK\_HOME=/home/XXX/tools/che/ddk/ddk**

    **export LD\_LIBRARY\_PATH=$DDK\_HOME/uihost/lib**

    >![](doc/source/img/icon-note.gif) **NOTE:**   
    >-   **XXX**  indicates the MindSpore Studio installation user, and  **/home/XXX/tools**  indicates the default installation path of the DDK.  
    >-   If the environment variables have been added, skip this step.  

    Enter  **:wq!**  to save and exit.

    Run the following command for the environment variable to take effect:

    **source \~/.bashrc**


## Deployment<a name="en-us_topic_0167511792_section1823144520529"></a>

1.  Access the root directory where the object detection application code is located as the MindSpore Studio installation user, for example,  **_/home/ascend/sample-objectdetection_**.
2.  Run the deployment script to prepare the project environment, including compiling and deploying the ascenddk public library and application.

    bash deploy.sh  _host\_ip_ _model\_mode_

    -   _host\_ip_: For the Atlas 200 DK developer board, this parameter indicates the IP address of the developer board.For the AI acceleration cloud server, this parameter indicates the IP address of the host.
    -   _model\_mode_  indicates the deployment mode of the model file. The default setting is  **internet**.
        -   **local**: If the Ubuntu system where MindSpore Studio is located is not connected to the network, use the local mode. In this case, download the dependent common code library ezdvpp to the  **sample-objectdetection/script**  directory by referring to the  [Downloading Network Models and and Dependency Code Library](#en-us_topic_0167511792_section13446115712539).
        -   **internet**: Indicates the online deployment mode. If the Ubuntu system where MindSpore Studio is located is connected to the network, use the Internet mode. In this case, download the dependency code library ezdvpp online.


    Example command:

    **bash deploy.sh 192.168.1.2**

3.  Upload the offline model file to be used and the image which requires inference to the directory of the  **HwHiAiUser**  user on the Host. For details, see  [Downloading Network Models and and Dependency Code Library](#en-us_topic_0167511792_section13446115712539).

    For example, upload the model file  **faster\_rcnn.om**  to the  **/home/HwHiAiUser/models**  directory on the host.
    
    The image requirements are as follows:
    - Format: JPG, PNG, and BMP.
    - Width of the input image: the value is an integer ranging from 16px to 4096px.
    - Height of the input image: the value is an integer ranging from 16px to 4096px.


## Running<a name="en-us_topic_0167511792_section1665916172539"></a>

1.  Log in to the Host as the  **HwHiAiUser**  user in SSH mode on Ubuntu Server where MindSpore Studio is located.

    **ssh HwHiAiUser@**_host\_ip_

    For the Atlas 200 DK, the default value of  _**host\_ip**_  is  **192.168.1.2**  \(USB connection mode\) or  **192.168.0.2**  \(NIC connection mode\).

    For the AI acceleration cloud server,  _**host\_ip**_  indicates the IP address of the server where MindSpore Studio is located.

2.  Go to the path of the executable file of Object Detection Application.

    **cd \~/HIAI\_PROJECTS/ascend\_workspace/objectdetection/out**

3.  Run the application.

    Run the  **run\_object\_detection\_faster\_rcnn.py**  script to save the images which are generated by inference to the specified path.

    Example command:

    **python3 run\_object\_detection\_faster\_rcnn.py -m  _\~/models/faster\_rcnn.om_  -w  _800_  -h  _600_  -i**

    **_./example.jpg_  -o  _./out_  -c _21_**

    -   **-m/--model\_path**: offline model path
    -   **-w/model\_width**: width of the input image of a model. The value is an integer ranging from 16 to 4096.
    -   **-h/model\_height**: height of the input image of a model. The value is an integer ranging from 16 to 4096.
    -   **-i/input\_path**: directory or path of the input image. You can enter multiple paths.
    -   **-o/output\_path**: directory for storing output images. The default setting is the current directory.
    -   **-c/output\_categories**: number of Faster R-CNN detection categories \(including the background\). The value is an integer ranging from 2 to 32. The default value is  **21**.

    For other parameters, run the  **python3 run\_object\_detection\_faster\_rcnn.py --help**  command. For details, see the help information.


## Downloading Network Models and and Dependency Code Library<a name="en-us_topic_0167511792_section13446115712539"></a>

-   Downloading network models

    The models used in the application are converted models that adapt to the Ascend 310 chipset. For details about how to download this kind of models and the original network models, see  [Table 1](#en-us_topic_0167511792_table0531392153). If you have a better model solution, you are welcome to share it at  [https://github.com/Ascend/models](https://github.com/Ascend/models).

     Upload the network models files (.om files) to the directory of the **HwHiAiUser** user on the Host.

    **Table  1**  Models used in Object Detection Application

    <a name="en-us_topic_0167511792_table0531392153"></a>
    <table><thead align="left"><tr id="en-us_topic_0167511792_row1154103991514"><th class="cellrowborder" valign="top" width="15.841584158415841%" id="mcps1.2.5.1.1"><p id="en-us_topic_0167511792_p195418397155"><a name="en-us_topic_0167511792_p195418397155"></a><a name="en-us_topic_0167511792_p195418397155"></a>Model Name</p>
    </th>
    <th class="cellrowborder" valign="top" width="21.782178217821784%" id="mcps1.2.5.1.2"><p id="en-us_topic_0167511792_p1054539151519"><a name="en-us_topic_0167511792_p1054539151519"></a><a name="en-us_topic_0167511792_p1054539151519"></a>Description</p>
    </th>
    <th class="cellrowborder" valign="top" width="28.425742574257427%" id="mcps1.2.5.1.3"><p id="en-us_topic_0167511792_p387083117108"><a name="en-us_topic_0167511792_p387083117108"></a><a name="en-us_topic_0167511792_p387083117108"></a>Model Download Path</p>
    </th>
    <th class="cellrowborder" valign="top" width="33.950495049504944%" id="mcps1.2.5.1.4"><p id="en-us_topic_0167511792_p35412397154"><a name="en-us_topic_0167511792_p35412397154"></a><a name="en-us_topic_0167511792_p35412397154"></a>Original Network Download Address</p>
    </th>
    </tr>
    </thead>
    <tbody><tr id="en-us_topic_0167511792_row4954262415"><td class="cellrowborder" valign="top" width="15.841584158415841%" headers="mcps1.2.5.1.1 "><p id="en-us_topic_0167511792_p1096112620413"><a name="en-us_topic_0167511792_p1096112620413"></a><a name="en-us_topic_0167511792_p1096112620413"></a>Network model for object detection</p>
    <p id="en-us_topic_0167511792_p1166611151750"><a name="en-us_topic_0167511792_p1166611151750"></a><a name="en-us_topic_0167511792_p1166611151750"></a>(<strong id="en-us_topic_0167511792_b3400194911919"><a name="en-us_topic_0167511792_b3400194911919"></a><a name="en-us_topic_0167511792_b3400194911919"></a>faster_rcnn.om</strong>)</p>
    </td>
    <td class="cellrowborder" valign="top" width="21.782178217821784%" headers="mcps1.2.5.1.2 "><p id="en-us_topic_0167511792_p69611263419"><a name="en-us_topic_0167511792_p69611263419"></a><a name="en-us_topic_0167511792_p69611263419"></a>This model is used in the <strong id="en-us_topic_0167511792_b1742163175610"><a name="en-us_topic_0167511792_b1742163175610"></a><a name="en-us_topic_0167511792_b1742163175610"></a>Object Detection</strong> application.</p>
    <p id="en-us_topic_0167511792_p135229539519"><a name="en-us_topic_0167511792_p135229539519"></a><a name="en-us_topic_0167511792_p135229539519"></a>It is a Faster R-CNN model based on Caffe.</p>
    </td>
    <td class="cellrowborder" valign="top" width="28.425742574257427%" headers="mcps1.2.5.1.3 "><p id="en-us_topic_0167511792_p10776202712619"><a name="en-us_topic_0167511792_p10776202712619"></a><a name="en-us_topic_0167511792_p10776202712619"></a>Download the model from the <strong id="en-us_topic_0167511792_b665726138"><a name="en-us_topic_0167511792_b665726138"></a><a name="en-us_topic_0167511792_b665726138"></a>computer_vision/object_detect/faster_rcnn</strong> directory in the <a href="https://github.com/Ascend/models/" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/models/</a> repository.</p>
    <p id="en-us_topic_0167511792_p87761327968"><a name="en-us_topic_0167511792_p87761327968"></a><a name="en-us_topic_0167511792_p87761327968"></a>For the version description, see the <strong id="en-us_topic_0167511792_b836252714314"><a name="en-us_topic_0167511792_b836252714314"></a><a name="en-us_topic_0167511792_b836252714314"></a>README.md</strong> file in the current directory.</p>
    </td>
    <td class="cellrowborder" valign="top" width="33.950495049504944%" headers="mcps1.2.5.1.4 "><p id="en-us_topic_0167511792_p147942911619"><a name="en-us_topic_0167511792_p147942911619"></a><a name="en-us_topic_0167511792_p147942911619"></a>For details, see the <strong id="en-us_topic_0167511792_b136123307430"><a name="en-us_topic_0167511792_b136123307430"></a><a name="en-us_topic_0167511792_b136123307430"></a>README.md</strong> file of the <strong id="en-us_topic_0167511792_b96121230134312"><a name="en-us_topic_0167511792_b96121230134312"></a><a name="en-us_topic_0167511792_b96121230134312"></a>computer_vision/object_detect/faster_rcnn</strong> directory in the <a href="https://github.com/Ascend/models/" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/models/</a> repository.</p>
    <p id="en-us_topic_0167511792_p1147911291361"><a name="en-us_topic_0167511792_p1147911291361"></a><a name="en-us_topic_0167511792_p1147911291361"></a></p>
    </td>
    </tr>
    </tbody>
    </table>

-   Download the dependent software libraries

    Download the dependent software libraries to the **sample-objectdetection/script** directory.

    **Table  2**  Download the dependent software library

    <a name="en-us_topic_0167511792_table141761431143110"></a>
    <table><thead align="left"><tr id="en-us_topic_0167511792_row18177103183119"><th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.1"><p id="en-us_topic_0167511792_p8177331103112"><a name="en-us_topic_0167511792_p8177331103112"></a><a name="en-us_topic_0167511792_p8177331103112"></a>Module Name</p>
    </th>
    <th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.2"><p id="en-us_topic_0167511792_p1317753119313"><a name="en-us_topic_0167511792_p1317753119313"></a><a name="en-us_topic_0167511792_p1317753119313"></a>Module Description</p>
    </th>
    <th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.2.4.1.3"><p id="en-us_topic_0167511792_p1417713111311"><a name="en-us_topic_0167511792_p1417713111311"></a><a name="en-us_topic_0167511792_p1417713111311"></a>Download Address</p>
    </th>
    </tr>
    </thead>
    <tbody><tr id="en-us_topic_0167511792_row19177133163116"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.1 "><p id="en-us_topic_0167511792_p2017743119318"><a name="en-us_topic_0167511792_p2017743119318"></a><a name="en-us_topic_0167511792_p2017743119318"></a>EZDVPP</p>
    </td>
    <td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.2 "><p id="en-us_topic_0167511792_p52110611584"><a name="en-us_topic_0167511792_p52110611584"></a><a name="en-us_topic_0167511792_p52110611584"></a>Encapsulates the dvpp interface and provides image and video processing capabilities, such as color gamut conversion and image / video conversion</p>
    </td>
    <td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.2.4.1.3 "><p id="en-us_topic_0167511792_p31774315318"><a name="en-us_topic_0167511792_p31774315318"></a><a name="en-us_topic_0167511792_p31774315318"></a><a href="https://github.com/Ascend/sdk-ezdvpp" target="_blank" rel="noopener noreferrer">https://github.com/Ascend/sdk-ezdvpp</a></p>
    <p id="en-us_topic_0167511792_p1634523015710"><a name="en-us_topic_0167511792_p1634523015710"></a><a name="en-us_topic_0167511792_p1634523015710"></a>After the download, keep the folder name <span class="filepath" id="en-us_topic_0167511792_filepath1324864613582"><a name="en-us_topic_0167511792_filepath1324864613582"></a><a name="en-us_topic_0167511792_filepath1324864613582"></a><b>ezdvpp</b></span>。</p>
    </td>
    </tr>
    </tbody>
    </table>


