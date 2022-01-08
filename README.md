# IELTS-Test-Activity-Portal
IELTS Test and Daily Activity based Student Online Portal.

## Overview
Project is all about creating IELTS training test and student daily activities through a online portal where student get logged in and hands on with the questions types and understand the test format going to came on the live computer based exam.

Challenge is to recreate exact replica of IELTS computer based test with same use cases and interactivity. Team consists of 2 member one Frontend developer/Designer and PHP Backend developer.


![IELTS TEST](https://github.com/umeshkathiriya/IELTS-Test-Activity-Portal/blob/main/Project_Snaps/User-Test-LandingPage.png?raw=true)

## Project walk through
Initial design has been created with theme branding of client **Door2Success** and functional prototype has been created with use of **HTML, CSS, Javascript, JQuery and Bootstrap**. Later **PHP backend** has been integrated so as user database can be managed and login sessions kept for Admin and User dashboard separately. Various functionality being developed for admin to check and give feedback on student activity and test submission.

IELTS test and daily activity, user interactivity logic,  validations, user answer comparison, and test/activity scoring has been written in JS and handle all possible scenario of language different question types in **Reading, Writing, and Listening**. Question types includes 
 - **Multi choice with single/Multiple answer**
 - **True/False/NotGiven**
 - **Fill the blanks**
 - **Matching headings**
 - **Drag&Drop single word/Phrase completion**
 - **Match Table (Matching information, features, labeling maps)**

All Question have predefined answer list matched with user submitted answer and final score will be awarded as **IELTS Band**.

Listening test are based on prerecorded audio that plan automatically and student need to complete the task while hearing. Listening test divided into **4 section** which automatically switch as audio progress and test automatically submit after **30 minutes** which is set timer for IELTS listening test.

Writing task developed for graph summary, letter, and essay writing with timer setup and word count. Students can write on given space or can submit the snap of hand written note on paper. Score for writing evaluate by admin (IELTS trainer) later from admin panel and score submitted will reflect on user dashboard.

## Code snippets
**Load audio track one after on completion and change the questions in section**
```javascript
/* Listening audio start */
if($('body').hasClass('activityListening') || $('body').hasClass('testListening')){
    audioobject = document.getElementById("audioTrack");
    var audio = document.getElementById("audioTrack");
    var index = 2;
    audio.play();
    audio.addEventListener('ended', function(){
        if(index <= 4){
            if($('body').hasClass('activityListening')){
                audio.src = '../activities/track/'+$('#slug').text()+'/s'+index+'.mp3';
            }else{
                audio.src = '../'+$('#slug').text().slice(0,-1)+'/track/s'+index+'.mp3';
            }
            /* console.log(audio.src); */ 
            audio.play();
            $('div[class^="para"], div[class^="section"]').css({'display':'none'});
            $('.para'+index).css({'display':'block'});
            $('.section'+index).css({'display':'block'});
            /* Activate the section first question */
            $('.test-part > ul header').each(function(){
                if($(this).text().match(/\d+/) == index){
                    $('.test-part > ul > li > a').parent().removeClass('selected');
                    $(this).next().addClass('selected');
                    $(this).next().children().addClass('active');
                    $('.test-listening').scrollTop(0);
                    focusAnswer();
                }
            });
            index++;
        }else{
            $('.btnSubmit').click();
        }
    });
}
/* End of listening */
```

**Compare the array of user answer and stored correct answer object in JSON format.**
```javascript
/* show answer in answer compare modal. */
var activityName = $('#slug').text(); /* get the name of activity */
var correctTotal = wrongTotal = 0;
var ansTotal = ansName.length;
ans.forEach(function(answer,i) {
    /* convert both answer to uppercase for easy matching words. */
    var actualAnswer = ansObject[activityName][i].toUpperCase();
    var submitAnswer = answer.toUpperCase();
    /* regex to match whole exact word/number submit and not null. */
    if(actualAnswer.match('\\b' + submitAnswer + '\\b','i') && answer.toUpperCase() != ''){
        $('#finalAnswer').append('<tr><td>'+ansName[i].match(/\d+/)[0]+'</td><td>'+answer+'</td><td>'+ansObject[activityName][i]+'</td></tr>');
        correctTotal++;
    }else{
        $('#finalAnswer').append('<tr><td class="danger">'+ansName[i].match(/\d+/)[0]+'</td><td class="danger">'+answer+'</td><td class="success">'+ansObject[activityName][i]+'</td></tr>');
        wrongTotal++;
    }
});
$('#correctTotal').append(correctTotal);
$('#ansTotal').append(ansTotal);
$('#wrongTotal').append(wrongTotal);
/* end of answer display */
/* call function to calculate the band score. */
bandScoreFn(correctTotal);
/* end of answer compare modal. */
```

**Native HTML5 Drag&Drop use case in one of question type in IELTS.**
```javascript
function drop(ev){
    ev.preventDefault();
    /* get current element id and OuterHTML */
    var data = ev.dataTransfer.getData("text"); 
    var currentTarget = ev.target;
    /* Check droptarget class and append else update the target */
    if (currentTarget.className == "dropTarget") {
        currentTarget.appendChild(document.getElementById(data));
        currentTarget.parentElement.classList.add("dropAnswered");
    }else{
        currentTarget.parentElement.appendChild(document.getElementById(data));
        currentTarget = currentTarget.parentElement;
    }
    /* remove the all dropover class from dropzone */
    document.querySelectorAll('.dropWrapper').forEach(e => e.classList.remove("dropOver"));
    /* remove and add first element from dropzone to dragzone */
    if(currentTarget.childElementCount > 1){
        var firstChildElement = currentTarget.firstElementChild.outerHTML;
        document.getElementById(dragList).innerHTML += firstChildElement;
        currentTarget.removeChild(currentTarget.childNodes[0]);
    }
    /* invoke jquery function */
    aqFunction(currentTarget.previousElementSibling);
}
```


Checkout the client website for detail insight: https://www.door2success.ca

Here project screenshots: [Project_Snaps](https://github.com/umeshkathiriya/IELTS-Test-Activity-Portal/tree/main/Project_Snaps)

**Note**: Detailed code can be shared on request.
