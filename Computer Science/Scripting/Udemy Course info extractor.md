This script extracts Udemy course sections titles and lecture titles, and prints it in a human-readable way, such that you can copy-paste it to later manipulate it

```javascript
(function() {
    let curriculumRoot = document.querySelector('div[data-purpose="course-curriculum"]');
    let sectionDivs = curriculumRoot ? curriculumRoot.querySelectorAll('div.ud-accordion-panel-toggler') : [];
    let readableList = '';
    
    Array.from(sectionDivs).forEach(section => {
        let titleElem = section.querySelector('.section--section-title--svpHP');
        let sectionTitle = titleElem ? titleElem.textContent.trim() : '';
        if(!sectionTitle) return;
        readableList += `\n=== ${sectionTitle} ===\n`;
        let panel = section.nextElementSibling;
        let liList = panel ? panel.querySelectorAll('li') : [];
        Array.from(liList).forEach(li => {
            let nameElem = li.querySelector('.lecture-title-text') || li;
            let lectureTitle = nameElem ? nameElem.textContent.slice(0, -5).replace('Preview', '').trim() : '';
    
    if (lectureTitle) readableList += `- ${lectureTitle}\n`;
        });
    });
    console.log(readableList.trim());
})();
```