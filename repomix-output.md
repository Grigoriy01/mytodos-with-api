This file is a merged representation of a subset of the codebase, containing specifically included files, combined into a single document by Repomix.

<file_summary>
This section contains a summary of this file.

<purpose>
This file contains a packed representation of a subset of the repository's contents that is considered the most important context.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.
</purpose>

<file_format>
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Repository files (if enabled)
5. Multiple file entries, each consisting of:
  - File path as an attribute
  - Full contents of the file
</file_format>

<usage_guidelines>
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.
</usage_guidelines>

<notes>
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Only files matching these patterns are included: src/**/*
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Files are sorted by Git change count (files with more changes are at the bottom)
</notes>

</file_summary>

<directory_structure>
src/
  api/
    todos.ts
  components/
    Footer/
      Footer.tsx
      index.ts
    Header/
      Header.tsx
      index.ts
    TodoItem/
      index.ts
      TodoItem.tsx
    TodoList/
      index.ts
      TodoList.tsx
  styles/
    animation.scss
    filter.scss
    index.scss
    todo.scss
    todoapp.scss
  types/
    Todo.ts
  utils/
    fetchClient.ts
  App.tsx
  index.tsx
  vite-env.d.ts
</directory_structure>

<files>
This section contains the contents of the repository's files.

<file path="src/api/todos.ts">
import { Todo } from '../types/Todo';
import { client } from '../utils/fetchClient';

export const USER_ID = 4347;

export const getTodos = () => {
  return client.get<Todo[]>(`/todos?userId=${USER_ID}`);
};

export const createTodo = (queryTitle: string) => {
  return client.post<Todo>(`/todos`, {
    title: queryTitle,
    userId: USER_ID,
    completed: false,
  });
};

export const deleteTodo = (todoId: number) => {
  return client.delete(`/todos/${todoId}`);
};

export const updateTodo = (todoId: number, data: Partial<Todo>) => {
  return client.patch<Todo>(`/todos/${todoId}`, data);
};
</file>

<file path="src/components/Footer/Footer.tsx">
import cn from 'classnames';

type Props = {
  countActiveTodos: number;
  activeFilter: string;
  hasCompletedTodos: boolean;
  onClearCompleted: () => void;
  filterStatus: {
    readonly ALL: 'all';
    readonly ACTIVE: 'active';
    readonly COMPLETED: 'completed';
  };
};

export const Footer: React.FC<Props> = ({
  countActiveTodos,
  activeFilter,
  hasCompletedTodos,
  onClearCompleted,
  filterStatus,
}) => {
  return (
    <footer className="todoapp__footer" data-cy="Footer">
      <span className="todo-count" data-cy="TodosCounter">
        {`${countActiveTodos} items left`}
      </span>

      <nav className="filter" data-cy="Filter">
        <a
          href="#/"
          className={cn('filter__link', {
            selected: activeFilter === filterStatus.ALL,
          })}
          data-cy="FilterLinkAll"
        >
          All
        </a>

        <a
          href="#/active"
          className={cn('filter__link', {
            selected: activeFilter === filterStatus.ACTIVE,
          })}
          data-cy="FilterLinkActive"
        >
          Active
        </a>

        <a
          href="#/completed"
          className={cn('filter__link', {
            selected: activeFilter === filterStatus.COMPLETED,
          })}
          data-cy="FilterLinkCompleted"
        >
          Completed
        </a>
      </nav>

      {/* this button should be disabled if there are no completed todos */}
      <button
        type="button"
        className="todoapp__clear-completed"
        data-cy="ClearCompletedButton"
        disabled={!hasCompletedTodos}
        onClick={() => onClearCompleted()}
      >
        Clear completed
      </button>
    </footer>
  );
};
</file>

<file path="src/components/Footer/index.ts">
export * from './Footer';
</file>

<file path="src/components/Header/Header.tsx">
import cn from 'classnames';

type Props = {
  isAllTodosCompleted: boolean;
  todosLength: number;
  onToggleAll: () => void;
  onAddTodo: (e: React.FormEvent<HTMLFormElement>) => void;
  inputRef: React.RefObject<HTMLInputElement> | null;
  query: string;
  setQuery: (e: string) => void;
  isLoading: boolean;
};

export const Header: React.FC<Props> = ({
  isAllTodosCompleted,
  todosLength,
  onToggleAll,
  onAddTodo,
  inputRef,
  query,
  setQuery,
  isLoading,
}) => {
  return (
    <header className="todoapp__header">
      {todosLength !== 0 && (
        <button
          type="button"
          className={cn('todoapp__toggle-all', {
            active: isAllTodosCompleted,
          })}
          data-cy="ToggleAllButton"
          onClick={() => onToggleAll()}
        />
      )}
      <form onSubmit={onAddTodo}>
        <input
          data-cy="NewTodoField"
          type="text"
          ref={inputRef}
          value={query}
          className="todoapp__new-todo"
          placeholder="What needs to be done?"
          onChange={e => setQuery(e.target.value)}
          disabled={isLoading}
        />
      </form>
    </header>
  );
};
</file>

<file path="src/components/Header/index.ts">
export * from './Header';
</file>

<file path="src/components/TodoItem/index.ts">
export * from './TodoItem';
</file>

<file path="src/components/TodoItem/TodoItem.tsx">
import { Todo } from '../../types/Todo';
import cn from 'classnames';

type Props = {
  todo: Todo;
  onToggleSingle: (todo: Todo) => void;
  onEditingTodoIdChange: (id: number | null) => void;
  onEditingTitleChange: (title: string) => void;
  editingTodoId: number | null;
  editingTitle: string;
  isProcessing: boolean;
  onDeleteTodo: (todoId: number) => void;
  onSubmit: (todo: Todo) => void;
};

export const TodoItem: React.FC<Props> = ({
  todo,
  onToggleSingle,
  onEditingTodoIdChange,
  onEditingTitleChange,
  editingTodoId,
  editingTitle,
  isProcessing,
  onDeleteTodo,
  onSubmit,
}) => {
  return (
    <div data-cy="Todo" className={cn('todo', { completed: todo.completed })}>
      <label className="todo__status-label" htmlFor={`todo__status-${todo.id}`}>
        <span className="is-sr-only">Toggle todo status</span>
        <input
          id={`todo__status-${todo.id}`}
          data-cy="TodoStatus"
          type="checkbox"
          className="todo__status"
          checked={todo.completed}
          onChange={() => onToggleSingle(todo)}
        />
      </label>

      {editingTodoId !== todo.id ? (
        <span
          data-cy="TodoTitle"
          className="todo__title"
          onDoubleClick={e => {
            e.preventDefault();
            onEditingTodoIdChange(todo.id);
            onEditingTitleChange(todo.title);
          }}
        >
          {todo.title}
        </span>
      ) : (
        <input
          className="todo__title-field"
          data-cy="TodoTitleField"
          type="text"
          value={editingTitle}
          onChange={e => onEditingTitleChange(e.target.value)}
          autoFocus
          placeholder={!editingTitle ? 'Empty todo will be deleted' : ''}
          onBlur={() => onSubmit(todo)}
          onKeyUp={e => {
            if (e.key === 'Enter') {
              onSubmit(todo);
            }

            if (e.key === 'Escape') {
              onEditingTodoIdChange(null);
            }
          }}
        />
      )}

      {editingTodoId !== todo.id && (
        <button
          type="button"
          className="todo__remove"
          data-cy="TodoDelete"
          onClick={() => onDeleteTodo(todo.id)}
        >
          ×
        </button>
      )}

      <div
        data-cy="TodoLoader"
        className={cn('modal overlay', { 'is-active': isProcessing })}
      >
        <div className="modal-background has-background-white-ter" />
        <div className="loader" />
      </div>
    </div>
  );
};
</file>

<file path="src/components/TodoList/index.ts">
export * from './TodoList';
</file>

<file path="src/components/TodoList/TodoList.tsx">
import React from 'react';
import { TransitionGroup, CSSTransition } from 'react-transition-group';
import cn from 'classnames';
import { TodoItem } from '../TodoItem';
import { Todo } from '../../types/Todo';

type Props = {
  tempTodo: Todo | null;
  isError: string | null;
  visibleTodos: Todo[];
  processingIds: number[];
  onToggleSingle: (todo: Todo) => void;
  onEditingTodoIdChange: (id: number | null) => void;
  onEditingTitleChange: (title: string) => void;
  editingTodoId: number | null;
  editingTitle: string;
  onDeleteTodo: (todoId: number) => void;
  onSubmit: (todo: Todo) => void;
};

export const TodoList: React.FC<Props> = ({
  tempTodo,
  isError,
  visibleTodos,
  processingIds,
  onToggleSingle,
  onEditingTodoIdChange,
  onEditingTitleChange,
  editingTodoId,
  editingTitle,
  onDeleteTodo,
  onSubmit,
}) => {
  return (
    <section
      className={cn('todoapp__main', { 'has-error': !!isError })}
      data-cy="TodoList"
    >
      <TransitionGroup>
        {visibleTodos.map(todo => {
          const isProcessing = processingIds.includes(todo.id);

          return (
            <CSSTransition key={todo.id} timeout={300} classNames="item">
              <TodoItem
                isProcessing={isProcessing}
                todo={todo}
                onToggleSingle={onToggleSingle}
                onEditingTodoIdChange={onEditingTodoIdChange}
                onEditingTitleChange={onEditingTitleChange}
                editingTodoId={editingTodoId}
                editingTitle={editingTitle}
                onDeleteTodo={onDeleteTodo}
                onSubmit={onSubmit}
              />
            </CSSTransition>
          );
        })}

        {tempTodo && (
          <CSSTransition key={0} timeout={300} classNames="temp-item">
            <div
              data-cy="Todo"
              className={cn('todo', { completed: tempTodo.completed })}
            >
              <label
                className="todo__status-label"
                htmlFor={`todo__status-temp`}
              >
                <span className="is-sr-only">Toggle todo status</span>
                <input
                  id={`todo__status-temp`}
                  data-cy="TodoStatus"
                  type="checkbox"
                  className="todo__status"
                  checked={false}
                  readOnly
                />
              </label>

              <span data-cy="TodoTitle" className="todo__title">
                {tempTodo.title}
              </span>

              <button
                type="button"
                className="todo__remove"
                data-cy="TodoDelete"
              >
                ×
              </button>

              <div data-cy="TodoLoader" className="modal overlay is-active">
                <div className="modal-background has-background-white-ter" />
                <div className="loader" />
              </div>
            </div>
          </CSSTransition>
        )}
      </TransitionGroup>
    </section>
  );
};
</file>

<file path="src/styles/animation.scss">
.item-enter-active {
    overflow: hidden;
    max-height: 58px;
    transition: max-height 0.5s ease-in-out;
  }

  .item-exit {
    max-height: 58px;
  }

  .item-exit-active {
    overflow: hidden;
    max-height: 0;
    transition: max-height 0.5s ease-in-out;
  }

  .temp-item-enter-active {
    overflow: hidden;
    max-height: 58px;
    transition: max-height 0.3s ease-in-out;
  }

  .temp-item-exit {
    max-height: 58px;
  }

  .temp-item-exit-active {
    transform: translateY(-58px);
    max-height: 0;
    opacity: 0;
    transition: 0.5s ease-in-out;
    transition-property: opacity, max-height, transform;
  }

  .has-error .temp-item-exit-active {
    transform: translateY(0);
    overflow: hidden;
  }
</file>

<file path="src/styles/filter.scss">
.filter {
  display: flex;

  &__link {
    margin: 3px;
    padding: 3px 7px;

    color: inherit;
    text-decoration: none;

    border: 1px solid transparent;
    border-radius: 3px;

    &:hover {
      border-color: rgba(175, 47, 47, 0.1);
    }

    &.selected {
      border-color: rgba(175, 47, 47, 0.2);
    }
  }
}
</file>

<file path="src/styles/index.scss">
iframe {
  display: none;
}

body {
  min-width: 230px;
  max-width: 550px;
  margin: 0 auto;
}

.notification {
  transition-property: opacity, min-height;
  transition-duration: 1s;
  min-height: 36px;
}

.notification.hidden {
  min-height: 0;
  opacity: 0;
  pointer-events: none;
}

@import "./todoapp";
@import "./todo";
@import "./filter";
@import "./animation";
</file>

<file path="src/styles/todo.scss">
.todo {
  position: relative;

  display: grid;
  grid-template-columns: 45px 1fr;
  justify-items: stretch;

  font-size: 24px;
  line-height: 1.4em;
  border-bottom: 1px solid #ededed;

  &:last-child {
    border-bottom: 0;
  }

  &__status-label {
    cursor: pointer;
    background-image: url("data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: center left;
  }

  &.completed &__status-label {
    background-image: url("data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E");
  }

  &__status {
    opacity: 0;
  }

  &__title {
    padding: 12px 15px;

    word-break: break-all;
    transition: color 0.4s;
  }

  &.completed &__title {
    color: #d9d9d9;
    text-decoration: line-through;
  }

  &__remove {
    position: absolute;
    right: 12px;
    top: 0;
    bottom: 0;

    font-size: 120%;
    line-height: 1;
    font-family: inherit;
    font-weight: inherit;
    color: #cc9a9a;

    float: right;
    border: 0;
    background: none;
    cursor: pointer;

    transform: translateY(-2px);
    opacity: 0;
    transition: color 0.2s ease-out;

    &:hover {
      color: #af5b5e;
    }
  }

  &:hover &__remove {
    opacity: 1;
  }

  &__title-field {
    width: 100%;
    padding: 11px 14px;

    font-size: inherit;
    line-height: inherit;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;

    border: 1px solid #999;
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);

    &::placeholder {
      font-style: italic;
      font-weight: 300;
      color: #e6e6e6;
    }
  }

  .overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 58px;

    opacity: 0.5;
  }
}

.item-enter {
  max-height: 0;
}

.item-enter-active {
  overflow: hidden;
  max-height: 58px;
  transition: max-height 0.3s ease-in-out;
}

.item-exit {
  max-height: 58px;
}

.item-exit-active {
  overflow: hidden;
  max-height: 0;
  transition: max-height 0.3s ease-in-out;
}

.temp-item-enter {
  max-height: 0;
}

.temp-item-enter-active {
  overflow: hidden;
  max-height: 58px;
  transition: max-height 0.3s ease-in-out;
}

.temp-item-exit {
  max-height: 58px;
}

.temp-item-exit-active {
  transform: translateY(-58px);
  max-height: 0;
  opacity: 0;
  transition: 0.3s ease-in-out;
  transition-property: opacity, max-height, transform;
}

.has-error .temp-item-exit-active {
  transform: translateY(0);
  overflow: hidden;
}
</file>

<file path="src/styles/todoapp.scss">
.todoapp {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 24px;
  font-weight: 300;
  color: #4d4d4d;
  margin: 40px 20px;

  &__content {
    margin-bottom: 20px;
    background: #fff;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
      0 25px 50px 0 rgba(0, 0, 0, 0.1);
  }

  &__title {
    font-size: 100px;
    font-weight: 100;
    text-align: center;
    color: rgba(175, 47, 47, 0.15);
    -webkit-text-rendering: optimizeLegibility;
    -moz-text-rendering: optimizeLegibility;
    text-rendering: optimizeLegibility;
  }

  &__header {
    position: relative;
  }

  &__toggle-all {
    position: absolute;

    height: 100%;
    width: 45px;

    display: flex;
    justify-content: center;
    align-items: center;

    font-size: 24px;
    color: #e6e6e6;

    border: 0;
    background-color: transparent;
    cursor: pointer;

    &.active {
      color: #737373;
    }

    &::before {
      content: "❯";
      transform: translateY(2px) rotate(90deg);
      line-height: 0;
    }
  }

  &__new-todo {
    width: 100%;
    padding: 16px 16px 16px 60px;

    font-size: 24px;
    line-height: 1.4em;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;

    border: none;
    background: rgba(0, 0, 0, 0.01);
    box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);

    &::placeholder {
      font-style: italic;
      font-weight: 300;
      color: #e6e6e6;
    }
  }

  &__main {
    border-top: 1px solid #e6e6e6;
  }

  &__footer {
    display: flex;
    justify-content: space-between;
    align-items: center;

    box-sizing: content-box;
    height: 20px;
    padding: 10px 15px;

    font-size: 14px;

    color: #777;
    text-align: center;
    border-top: 1px solid #e6e6e6;

    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
      0 8px 0 -3px #f6f6f6,
      0 9px 1px -3px rgba(0, 0, 0, 0.2),
      0 16px 0 -6px #f6f6f6,
      0 17px 2px -6px rgba(0, 0, 0, 0.2);
  }

  &__clear-completed {
    margin: 0;
    padding: 0;
    border: 0;

    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    text-decoration: none;

    cursor: pointer;
    background: none;

    -webkit-appearance: none;
    appearance: none;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    transition: opacity 0.3s;

    &:hover {
      text-decoration: underline;
    }

    &:active {
      text-decoration: none;
    }

    &:disabled {
      visibility: hidden;
    }
  }
}
</file>

<file path="src/types/Todo.ts">
export interface Todo {
  id: number;
  userId: number;
  title: string;
  completed: boolean;
}
</file>

<file path="src/utils/fetchClient.ts">
/* eslint-disable @typescript-eslint/no-explicit-any */
const BASE_URL = 'https://mate.academy/students-api';

// returns a promise resolved after a given delay
function wait(delay: number) {
  return new Promise(resolve => {
    setTimeout(resolve, delay);
  });
}

// To have autocompletion and avoid mistypes
type RequestMethod = 'GET' | 'POST' | 'PATCH' | 'DELETE';

function request<T>(
  url: string,
  method: RequestMethod = 'GET',
  data: any = null, // we can send any data to the server
): Promise<T> {
  const options: RequestInit = { method };

  if (data) {
    // We add body and Content-Type only for the requests with data
    options.body = JSON.stringify(data);
    options.headers = {
      'Content-Type': 'application/json; charset=UTF-8',
    };
  }

  // DON'T change the delay it is required for tests
  return wait(100)
    .then(() => fetch(BASE_URL + url, options))
    .then(response => {
      if (!response.ok) {
        throw new Error();
      }

      return response.json();
    });
}

export const client = {
  get: <T>(url: string) => request<T>(url),
  post: <T>(url: string, data: any) => request<T>(url, 'POST', data),
  patch: <T>(url: string, data: any) => request<T>(url, 'PATCH', data),
  delete: (url: string) => request(url, 'DELETE'),
};
</file>

<file path="src/App.tsx">
import React, { useEffect, useState } from 'react';
import cn from 'classnames';
import {
  getTodos,
  USER_ID,
  createTodo,
  deleteTodo,
  updateTodo,
} from './api/todos';
import { Todo } from './types/Todo';
import { Footer } from './components/Footer';
import { Header } from './components/Header';
import { TodoList } from './components/TodoList';

const FILTER_STATUS = {
  ALL: 'all',
  ACTIVE: 'active',
  COMPLETED: 'completed',
} as const;

export const App: React.FC = () => {
  //#region States
  const [todos, setTodos] = useState<Todo[]>([]);
  const [tempTodo, setTempTodo] = useState<Todo | null>(null);
  const [activeFilter, setActiveFilter] = useState<string>(FILTER_STATUS.ALL);
  const [editingTitle, setEditingTitle] = useState('');
  const [editingTodoId, setEditingTodoId] = useState<number | null>(null);
  const [query, setQuery] = useState('');
  const [isError, setIsError] = useState<string | null>(null);
  const [isLoading, setIsLoading] = useState(false);
  const [processingIds, setProcessingIds] = useState<number[]>([]);
  //#endregion States

  //#region useEffect
  /**
   * Effect hook for loading todos from the API on component mount.
   */
  useEffect(() => {
    let ignore = false;
    /**
     * Loads todos from the API and updates the component state.
     * Handles loading, success, and error states.
     */

    const loadTodos = async () => {
      setIsLoading(true);

      try {
        const dataTodos = await getTodos();

        if (!ignore) {
          setTodos(dataTodos);
          setIsError(null);
          setTempTodo(null);
        }
      } catch {
        if (ignore) {
          return;
        }

        setIsError('Unable to load todos');
      } finally {
        if (!ignore) {
          setIsLoading(false);
        }
      }
    };

    loadTodos();

    return () => {
      ignore = true;
    };
  }, []);

  /**
   * Effect hook to manage the display duration of error messages.
   * Clears the error message after 3 seconds.
   */
  useEffect(() => {
    if (!isError) {
      return;
    }

    const timerErrorId = setTimeout(() => setIsError(null), 3000);

    return () => {
      clearTimeout(timerErrorId);
    };
  }, [isError]);

  const inputRef = React.useRef<HTMLInputElement>(null);

  /**
   * Effect hook to focus the new todo input field whenever the todos array changes.
   */
  useEffect(() => {
    const timerId = setTimeout(() => {
      inputRef.current?.focus();
    }, 0);

    return () => {
      clearTimeout(timerId);
    };
  }, [isLoading, todos.length]);

  useEffect(() => {
    const handleHashChange = () => {
      const hash = window.location.hash;

      if (hash === '#/active') {
        setActiveFilter(FILTER_STATUS.ACTIVE);
      } else if (hash === '#/completed') {
        setActiveFilter(FILTER_STATUS.COMPLETED);
      } else {
        setActiveFilter(FILTER_STATUS.ALL);
      }
    };

    handleHashChange();

    window.addEventListener('hashchange', handleHashChange);

    return () => {
      window.removeEventListener('hashchange', handleHashChange);
    };
  }, [activeFilter]);
  //#endregion useEffect

  //#region ----------------functions-----------------
  /**
   * Filters the list of todos based on the provided filter type.
   * @param {string} filter_type - The type of filter to apply ('all', 'active', 'completed').
   * @returns {Todo[]} An array of todos filtered according to the specified type.
   */
  const getFilteredTodos = (filter_type: string) => {
    switch (filter_type) {
      case FILTER_STATUS.ACTIVE:
        return todos.filter(todo => !todo.completed);
      case FILTER_STATUS.COMPLETED:
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  };

  /**
   * Handles the submission of the add todo form.
   * Creates a new todo, adds it to the list, and handles loading and error states.
   * @param {React.FormEvent<HTMLFormElement>} event - The form submission event.
   */

  const handleAddTodo = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    const trimmedQuery = query.trim();

    if (!trimmedQuery) {
      setIsError('Title should not be empty');
      setQuery('');

      return;
    }

    setIsError(null);
    setIsLoading(true);

    const emptyTodo = {
      id: 0,
      title: trimmedQuery,
      userId: USER_ID,
      completed: false,
    };

    setTempTodo(emptyTodo);

    try {
      const newTodo = await createTodo(trimmedQuery);

      setTodos(prev => [...prev, newTodo]);
      setQuery('');
    } catch {
      setIsError('Unable to add a todo');
    } finally {
      setTempTodo(null);
      setIsLoading(false);
    }
  };

  /**
   * Handles clearing all completed todos.
   */
  const handleClearCompleted = async () => {
    const completedTodos = todos.filter(todo => todo.completed);
    const completedTodosId = completedTodos.map(todo => todo.id);

    setProcessingIds(prev => [...prev, ...completedTodosId]);

    try {
      await Promise.all(
        completedTodos.map(async todo => {
          try {
            await deleteTodo(todo.id);
            setTodos(prev => prev.filter(t => t.id !== todo.id));
          } catch {
            throw new Error('Failed to delete');
          }
        }),
      );
      //setTodos(prev => prev.filter(todo => !todo.completed));
    } catch {
      setIsError('Unable to delete a todo');
    } finally {
      setProcessingIds(prev =>
        prev.filter(id => !completedTodosId.includes(id)),
      );
      inputRef.current?.focus();
    }
  };

  /**
   * Handles deleting a specific todo by its ID.
   * @param {number} todoId - The ID of the todo to delete.
   */
  const handleDeleteTodo = async (todoId: number) => {
    setProcessingIds(prev => [...prev, todoId]);

    try {
      await deleteTodo(todoId);
      setTodos(prev => prev.filter(todo => todo.id !== todoId));
    } catch (error) {
      setIsError('Unable to delete a todo');
      throw error;
    } finally {
      setProcessingIds(prev => prev.filter(id => id !== todoId));
    }
  };

  const handleToggleSingle = async (todo: Todo) => {
    setProcessingIds(prev => [...prev, todo.id]);

    try {
      const newTodo = await updateTodo(todo.id, {
        ...todo,
        completed: !todo.completed,
      });

      setTodos(prev => prev.map(t => (t.id === newTodo.id ? newTodo : t)));
    } catch {
      setIsError('Unable to update a todo');
    } finally {
      setProcessingIds(prev => prev.filter(id => id !== todo.id));
    }
  };

  const handleToggleAll = async () => {
    const hasActiveTodo = todos.some(todo => !todo.completed);
    const todosUpdate = todos.filter(todo => todo.completed !== hasActiveTodo);
    const idsTodoUpdate = todosUpdate.map(todo => todo.id);

    setProcessingIds(prev => [...prev, ...idsTodoUpdate]);

    try {
      await Promise.all(
        todosUpdate.map(async todo => {
          const newTodo = await updateTodo(todo.id, {
            ...todo,
            completed: hasActiveTodo,
          });

          setTodos(prev => prev.map(t => (t.id === newTodo.id ? newTodo : t)));
        }),
      );
    } catch {
      setIsError('Unable to update a todo');
    } finally {
      setProcessingIds(prev => prev.filter(id => !idsTodoUpdate.includes(id)));
    }
  };

  const handleEditingTodo = async (todo: Todo, text: string) => {
    const trimmedText = text.trim();

    setProcessingIds(prev => [...prev, todo.id]);

    try {
      const editedTodo = await updateTodo(todo.id, {
        ...todo,
        title: trimmedText,
      });

      setTodos(prev =>
        prev.map(t => (editedTodo.id === t.id ? editedTodo : t)),
      );
      setEditingTodoId(null);
    } catch {
      setIsError('Unable to update a todo');
    } finally {
      setProcessingIds(prev => prev.filter(id => todo.id !== id));
    }
  };

  const handleSubmit = async (todo: Todo) => {
    const trimmed = editingTitle.trim();

    if (!trimmed) {
      try {
        await handleDeleteTodo(todo.id);
      } catch {}

      return;
    }

    if (todo.title !== editingTitle) {
      await handleEditingTodo(todo, trimmed);
    } else {
      setEditingTodoId(null);
    }
  };
  //#endregion functions

  const isAllTodosCompleted =
    todos.length > 0 && todos.every(todo => todo.completed);
  const hasCompletedTodos = todos.some(todo => todo.completed);
  const todosLength = todos.length;

  const countActiveTodos = todos.filter(todo => !todo.completed).length;
  const visibleTodos = getFilteredTodos(activeFilter);

  //#region rendering
  return (
    <div className="todoapp">
      <h1 className="todoapp__title">todos</h1>

      <div className="todoapp__content">
        <Header
          todosLength={todosLength}
          isAllTodosCompleted={isAllTodosCompleted}
          onToggleAll={handleToggleAll}
          onAddTodo={handleAddTodo}
          inputRef={inputRef}
          query={query}
          setQuery={setQuery}
          isLoading={isLoading}
        />
        <TodoList
          visibleTodos={visibleTodos}
          tempTodo={tempTodo}
          processingIds={processingIds}
          editingTodoId={editingTodoId}
          editingTitle={editingTitle}
          onDeleteTodo={handleDeleteTodo}
          onEditingTodoIdChange={setEditingTodoId}
          onEditingTitleChange={setEditingTitle}
          onToggleSingle={handleToggleSingle}
          onSubmit={handleSubmit}
          isError={isError}
        />

        {todos.length !== 0 && (
          <Footer
            countActiveTodos={countActiveTodos}
            activeFilter={activeFilter}
            hasCompletedTodos={hasCompletedTodos}
            onClearCompleted={handleClearCompleted}
            filterStatus={FILTER_STATUS}
          />
        )}
      </div>

      <div
        data-cy="ErrorNotification"
        className={cn(
          'notification is-danger is-light has-text-weight-normal',
          { hidden: !isError },
        )}
      >
        <button
          data-cy="HideErrorButton"
          type="button"
          className="delete"
          onClick={() => setIsError(null)}
        />
        {isError}
      </div>
    </div>
  );
  //#endregion rendering
};
</file>

<file path="src/index.tsx">
import { createRoot } from 'react-dom/client';

import 'bulma/css/bulma.css';
import '@fortawesome/fontawesome-free/css/all.css';
import './styles/index.scss';

import { App } from './App';

createRoot(document.getElementById('root') as HTMLDivElement).render(<App />);
</file>

<file path="src/vite-env.d.ts">
/// <reference types="vite/client" />
</file>

</files>
