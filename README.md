# Technical task - Quiz

You can view the full assignment [HERE](./Front-end_Developer_Technical_task.pdf)

You can see a working project here - asdfasdfsad

![Project GIF](./frontend-quiz.gif)

## Project architecture

1. File arrangement:
   - The entry point for all quiz related components is Quiz.vue
   - The 3 quiz views - form, test, results - are separated into different components
   - Shared components, e.g. button, progress bar, loader, test answer, are separate components
   - This could be improved further by separating form fields, etc.
2. Quiz component:
   - The parent Quiz component holds state for which view to render, 
   submitted quiz form values and submitted test answers, as well as their respective methods.
3. QuizForm component:
   - Holds quiz form and validation. If form field is empty the form can't be submitted.
   - On component render the name input field is focused
   - Test options are fetched from API using AXIOS
4. QuizTest component:
   - Holds state for all chose test questions, active question index, 
   active question answers and selected answer
   - All questions are fetched on initial render
   - Answers are fetched for each question separately
   - User can't proceed to next question if an answer hasn't been selected
   - On each question the answer is being added to the Quiz components answers state
5. QuizResults component:
   - Receives users name, test id and answers as props
   - Results are retrieved from API

---

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
