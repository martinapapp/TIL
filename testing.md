# Testing

## Index

[]()

## Testing tools and setup

**Tools:**

- Vitest : testing framework (provide us a tes runner & functions to orgenizing test)
- React Testing Library : to render React components, add DOM matchers, initiate user events
- jsdom : install DOM environment to run tests

**Steps:**

1. npm install -D vitest
2. npm install -D @testing-library/react @testing-library/jest-dom @testing-library/user-event
3. npm install -D jsdom
4. touch root/test-setup.js
    ```
    import "testing-library/jest-dom/vitest"
    import {afterEach} from "vitest"
    import {cleanup} from "@testing-library/react"

    afterEach(() => cleanup())
    ```
5. vite.config.js
    ```
    test:{
        setupFiles: ['./test-setup.js'],
        environment: 'jsdom'
    }
    ```
6. package.json scripts:
    ```
    "test":"vitest"
    ```
7. npm run test

## Base tests and imports

    1. name the the test file: `ComponentName.test.jsx`
    2. imports
    ```
    import {test, expect, describe} from 'vitest'
    import {render, screen} from '@testing-library/react'
    ```
    3.1. tests
    ```
    describe("ComponentName", () => {

        test("describe test", () => {
            render(<ComponentName />)
            expect(screen.getByText('expected text')).toBeInTheDocument()
        })
        test("describe test", () => {
            render(<ComponentName />)
            expect(screen.getByRole('expected role eg: img').src)toContain("img name")
        })

    })
    ```
    3.2. tests
    ```
    describe("ComponentName", () => {
        test('describe tests', () => {
            render(<ComponentName />)
            expect(screen.getByText("exact text")).toBeInTheDocument()
            expect(screen.getByPlaceholderText("exact text")).toBeInTheDocument()
            expect(screen.getByRole('role').textContent).toBe('text content')
        })
    })
    ```

## Test User Interaction 

- pattern : Arrange Act Assert
    - Arrange = preconditons and inputs (mimic user)
    - Act = act on the obj or merhod under test (mimic user interaction)
    - Assert = expected results have occured or not (check)
- import: `import {userEvent} from '@testing-library/user-event'`
- steps:
    1. **Arrange**
        ```
        const user = userEvent.setup()   //create a user
        render(<ComponentName />)
        const testName = screen.getAllByRole('role')[0]  //query role
        ```
    2. **Act**
        ```
        await user.clear(testName)  //promise to clear content
        await user.type(testName, "the exact content") 
        ```
    3. **Assert**
        ```
        expect(screen.getByText("exact content")).toBeInTheDocument()   //check
        ```

## Test external services

    ```
    test("describe test", async() => {

        //Arrange
        const user = userEvent.setup()
        render(<ComponentName />)
        const testName = screen.getByRole('role')

        //Act
        await user.click(testName)

        //Assert
        const images = screen.getAllByRole('img')
        expect(images[1].src).toBe('link')

    })
    ```

## Code Coverage

Metric tool, how much of the code is executed during the testing.
Vitest code coverage tools: v8 (default), istanbul.

**Steps**
1. install : `npm install @vitest/coverage/istanbul`
2. package.json script : `"test-coverage": "vitest --coverage"`
3. vite.config.js test : 
```
import {coverageConfigDefaults} from 'vite/config'

coverage:{
    provider : 'istanbul'
    exclude : [...coverageConfigDefault.exclude]  // if need to
}
```
4. npm run test:coverage

## Test A11y with TDD

- TDD = test driven development
- first file test then write missing code then refactor to improve code
- add test codes in root/a11y.test.jsx

## Test Types

*How?* 
- Manual
- Automated

*What?*
- Non-functional (a11y, compatibility, performance, stress, load, security)
- Functional (unit testing, component, integration, end-to-en, system , UI, acceptance, API, contract)

*How much and Why?*
- Regression
- Smoke

*At what level of abstraction?*
- Black Box
- Gray Box
- White Box
